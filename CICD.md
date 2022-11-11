Kết nối Gitlab và Jenkins
[Jenkins Tutorial: Cấu hình webhook cho Gitlab repo](https://www.youtube.com/watch?v=ZbSfspvzyC0&list=PLlahAO-uyDzJ7sWdvD_j0eyvbKtf-1quq&index=5)

[Jenkins Tutorial: Kết nối Gitlab repo với Jenkins Pipeline](https://www.youtube.com/watch?v=otCfuA40dTs&list=PLlahAO-uyDzJ7sWdvD_j0eyvbKtf-1quq&index=6)

## Cài đặt Jenkins thông qua docker
```
docker run -d  -p 8080:8080 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  --name jenkins \
  jenkins/jenkins:lts-jdk11
```
```
docker exec -it -u root jenkins bash
```
```
apt-get update && \
apt-get -y install apt-transport-https \
     ca-certificates \
     curl \
     gnupg2 \
     software-properties-common && \
curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg > /tmp/dkey; apt-key add /tmp/dkey && \
add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
   $(lsb_release -cs) \
   stable" && \
apt-get update && \
apt-get -y install docker-ce
```
```
docker ps
```
```
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

### Chú ý: Mục lưu trữ images trong jenkins container
```
docker exec -it -u 0 c562bc1cd409 bin/bash
```
```
root@c562bc1cd409:/var/jenkins_home/workspace
```
#### Docker pipe line
>Dashboard > Manage Jenkins > Manage Plugins > Available (tab) > docker-workflow

### Cài dotnet CLI trong Jenkins

FROM jenkins/jenkins:lts
#### Switch to root to install .NET Core SDK
USER root

#### Just for my sanity... Show me this distro information!
RUN uname -a && cat /etc/*release

# Based on instructiions at https://learn.microsoft.com/en-us/dotnet/core/linux-prerequisites?tabs=netcore2x
# Install depency for dotnet core 2.
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    curl libunwind8 gettext apt-transport-https && \
    curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg && \
    mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg && \
    sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-debian-stretch-prod stretch main" > /etc/apt/sources.list.d/dotnetdev.list' && \
    apt-get update

#### Install the .Net Core framework, set the path, and show the version of core installed.
RUN apt-get install -y dotnet-sdk-2.0.0 && \
    export PATH=$PATH:$HOME/dotnet && \
    dotnet --version

#### Good idea to switch back to the jenkins user.
USER jenkins
```
sudo chown -R jenkins:jenkins /tmp/NuGetScratch/
```
=> Tránh lỗi error : Unable to obtain lock file access on

#### Update package nudget cho docker trong jenkins
```
cd ~/.nuget/packages
```
copy package:
```
cp -R tms.flexcel.webforms ~/.nuget/packages
```
```
 kubectl create secret docker-registry regcred --docker-server=reg.mandalaholiday.tk --docker-username=k8s --docker-password=Apec@123 --docker-email=huybka1996@gmail.com
 ```
 ```
 kubectl get secret regcred --output=yaml
 ```
 output:

apiVersion: v1
data:
  .dockerconfigjson: eyJhdXRocyI6eyJyZWcubWFuZGFsYWhvbGlkYXkudGsiOnsidXNlcm5hbWUiOiJrOHMiLCJwYXNzd29yZCI6IkFwZWNAMTIzIiwiZW1haWwiOiJodXlia2ExOTk2QGdtYWlsLmNvbSIsImF1dGgiOiJhemh6T2tGd1pXTkFNVEl6In19fQ==
kind: Secret
metadata:
  creationTimestamp: "2022-11-09T16:12:30Z"
  name: regcred
  namespace: default
  resourceVersion: "6660"
  uid: b76f6aa8-2b9d-4713-9fbe-2a030a119b4d


Lấy file config k8s
```
 cd .kube
 cat config
 ```

 Jenkinsfile với Aspnetcore
 ```jenkinsfile
     //  Gitlab
	def gitRepository = 'https://gitlab.com/HuyNV1996/holiday-booking-aspnet.git'
	def gitBranch = 'main'
	def gitlabCredential = 'holiday-booking-aspnet'

    // Harbor
	def imageGroup = 'holiday-booking'
	def appName = "admin"
	def namespace = "booking-aspnet-core"	
	def registryCredential = 'jenkin_harbor'
	def VERSION  = "prod-0.${BUILD_NUMBER}"
	
	pipeline {
		agent any
		environment {
			DOCKER_REGISTRY = 'https://reg.mandalaholiday.tk'
			DOCKER_IMAGE_NAME = "${imageGroup}/${appName}"
			DOCKER_IMAGE = "reg.mandalaholiday.tk/${DOCKER_IMAGE_NAME}"
	    }		
		stages {
		    stage("Verify tooling"){
    		    steps {
    		        sh'''
    		        docker info
    		        curl --version
    		        '''
    		    }
		    }
			stage('Git Checkout') 
			{
			  steps 
			  {
			    checkout(
			        [$class: 'GitSCM',
			        branches: [[name: '*/main']],
			        extensions: [[$class: 'CloneOption', noTags: false, reference: '', shallow: false, timeout: 120]],
			        userRemoteConfigs: [[credentialsId: gitlabCredential, url: gitRepository]]])
				    sh "git reset --hard"				
			  }
			}
			stage('Build and Push Images')
            {
                steps 
    			{
                     script{
				        sh '''#!/bin/bash
				        dotnet restore "./MandalaBooking.sln"
                        dotnet publish /nr:false "./module-admin/HttpApi.Host/MandalaBooking.Admin.HttpApi.Host.csproj" -c Release -o ./module-admin/HttpApi.Host/published
                        dotnet publish /nr:false "./module-danh-muc/HttpApi.Host/MandalaBooking.DanhMuc.HttpApi.Host.csproj" -c Release -o ./module-danh-muc/HttpApi.Host/published
                        dotnet publish /nr:false "./module-import-export/HttpApi.Host/MandalaBooking.ImportExport.HttpApi.Host.csproj" -c Release -o ./module-import-export/HttpApi.Host/published
                        
                        cp -R ./module-admin/OrdFull.HttpApi.Host/Resources ./module-admin/HttpApi.Host/published
                        
                        docker rmi reg.mandalaholiday.tk/holiday-booking/admin -f
                        docker rmi reg.mandalaholiday.tk/holiday-booking/danhmuc -f
                        docker rmi reg.mandalaholiday.tk/holiday-booking/im-export -f
				        '''
				        admin = docker.build('reg.mandalaholiday.tk/holiday-booking/admin', './module-admin/HttpApi.Host')
						docker.withRegistry( DOCKER_REGISTRY, registryCredential ) {                       
						    admin.push()
						}
						
						danhmuc = docker.build('reg.mandalaholiday.tk/holiday-booking/danhmuc', './module-danh-muc/HttpApi.Host')
						docker.withRegistry( DOCKER_REGISTRY, registryCredential ) {                       
						    danhmuc.push()
                        }
                        
                        import_export = docker.build('reg.mandalaholiday.tk/holiday-booking/im-export', './module-import-export/HttpApi.Host')
						docker.withRegistry( DOCKER_REGISTRY, registryCredential ) {                       
						    import_export.push()
                        }
                    }
                   
                }
		    }
		  //  stage('Clear Folder'){
		  //      steps 
    // 			{
    //                  script{
    //         		        sh '''
    //         		        chmod -R 777 ./module-admin/HttpApi.Host/published
    //         		        chmod -R 777 ./module-danh-muc/HttpApi.Host/published
    //         		        chmod -R 777 ./module-import-export/HttpApi.Host/published
    //         		        rm -f -R ./module-admin/HttpApi.Host/published
    //         		        rm -f -R ./module-danh-muc/HttpApi.Host/published
    //         		        rm -f -R ./module-import-export/HttpApi.Host/published
    //         		        '''
    //                  }
    // 			}
		  //  }
		}
	}
 ```
 Jenkinsfile với Angular
 ```
 //  Gitlab
	def gitRepository = 'https://gitlab.com/HuyNV1996/holiday-booking-angular.git'
	def gitBranch = 'main'
	def gitlabCredential = 'holiday-booking-angular'

    // Harbor
	def imageGroup = 'holiday-booking'
	def appName = "angular"
	def namespace = "booking-angular"	
	def registryCredential = 'jenkin_harbor'
	def VERSION  = "prod-0.${BUILD_NUMBER}"
	
	pipeline {
		agent any
		environment {
			DOCKER_REGISTRY = 'https://reg.mandalaholiday.tk'
			DOCKER_IMAGE_NAME = "${imageGroup}/${appName}"
			DOCKER_IMAGE = "reg.mandalaholiday.tk/${DOCKER_IMAGE_NAME}"
	    }		
		stages {
			stage('Git Checkout') 
			{
			  steps 
			  {
			    checkout(
			        [$class: 'GitSCM',
			        branches: [[name: '*/main']],
			        extensions: [[$class: 'CloneOption', noTags: false, reference: '', shallow: false, timeout: 120]],
			        userRemoteConfigs: [[credentialsId: gitlabCredential, url: gitRepository]]])
				    sh "git reset --hard"				
			  }
			}
			stage("Build and Push Images"){
    		    steps {
    		        script{
				        sh '''#!/bin/bash
        		        cd ./ClientApp
        		        yarn
        		        yarn run build:prod
        		        node --max_old_space_size=12000 ./node_modules/@angular/cli/bin/ng build --configuration production
        		        docker rmi reg.mandalaholiday.tk/holiday-booking/angular -f
        		        '''
        		        angular = docker.build('reg.mandalaholiday.tk/holiday-booking/angular', './ClientApp')
    					docker.withRegistry( DOCKER_REGISTRY, registryCredential ) {                       
    						    angular.push()
    					}
        		    }
    		    }
		    }
		    stage('Deploy to K8s')
            {
               steps{
                    script{
                        
                        kubernetesDeploy (configs: './home/admins/booking-k8s/mandala-hotel-angular-deployment.yaml',kubeconfigId: 'jenkins_k8s_uat')
                    }
                }
            }
		}
	}
 ```