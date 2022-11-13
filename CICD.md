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
 ```
    //  Gitlab
	def gitRepository = 'https://gitlab.com/HuyNV1996/holiday-backoffice-aspnetcore.git'
	def gitBranch = 'main'
	def gitlabCredential = 'holiday-backoffice-aspnet'

    // Harbor
	def imageGroup = 'holiday-backoffice'
	def appName = "admin"
	def namespace = "backoffice-aspnet-core"	
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
			stage('Build Docker Images')
            {
                steps 
    			{
                     script{
				        sh '''#!/bin/bash
				        dotnet restore "./MandalaHotelBackOffice.sln"
                        dotnet publish /nr:false "./module-admin/HttpApi.Host/MandalaHotelBackOffice.Admin.HttpApi.Host.csproj" -c Release -o ./module-admin/HttpApi.Host/published
                        dotnet publish /nr:false "./module-danh-muc/HttpApi.Host/MandalaHotelBackOffice.DanhMuc.HttpApi.Host.csproj" -c Release -o ./module-danh-muc/HttpApi.Host/published
                        dotnet publish /nr:false "./module-import-export/HttpApi.Host/MandalaHotelBackOffice.ImportExport.HttpApi.Host.csproj" -c Release -o ./module-import-export/HttpApi.Host/published
                        dotnet publish /nr:false "./module-customer/HttpApi.Host/MandalaHotelBackOffice.Customer.HttpApi.Host.csproj" -c Release -o ./module-customer/HttpApi.Host/published
                        dotnet publish /nr:false "./module-contract/HttpApi.Host/MandalaHotelBackOffice.Contract.HttpApi.Host.csproj" -c Release -o ./module-contract/HttpApi.Host/published
                        dotnet publish /nr:false "./module-booking/HttpApi.Host/MandalaHotelBackOffice.Booking.HttpApi.Host.csproj" -c Release -o ./module-booking/HttpApi.Host/published
                        
                        cp -R ./module-admin/OrdFull.HttpApi.Host/Resources ./module-admin/HttpApi.Host/published
                        
                        docker rmi reg.mandalaholiday.tk/holiday-backoffice/admin -f
                        docker rmi reg.mandalaholiday.tk/holiday-backoffice/danhmuc -f
                        docker rmi reg.mandalaholiday.tk/holiday-backoffice/im-export -f
                        docker rmi reg.mandalaholiday.tk/holiday-backoffice/customer -f
                        docker rmi reg.mandalaholiday.tk/holiday-backoffice/contract -f
                        docker rmi reg.mandalaholiday.tk/holiday-backoffice/booking -f
				        '''
				        admin = docker.build('reg.mandalaholiday.tk/holiday-backoffice/admin', './module-admin/HttpApi.Host')
						danhmuc = docker.build('reg.mandalaholiday.tk/holiday-backoffice/danhmuc', './module-danh-muc/HttpApi.Host')
                        import_export = docker.build('reg.mandalaholiday.tk/holiday-backoffice/im-export', './module-import-export/HttpApi.Host')
                        customer = docker.build('reg.mandalaholiday.tk/holiday-backoffice/customer', './module-customer/HttpApi.Host')
                        contract = docker.build('reg.mandalaholiday.tk/holiday-backoffice/contract', './module-contract/HttpApi.Host')
                        booking = docker.build('reg.mandalaholiday.tk/holiday-backoffice/booking', './module-booking/HttpApi.Host')
                    }
                }
		    }
        stage('Push Docker Image') {
          steps {
            script {
              docker.withRegistry(DOCKER_REGISTRY, registryCredential) {
                admin.push("latest")
              }
              docker.withRegistry(DOCKER_REGISTRY, registryCredential) {
                danhmuc.push("latest")
              }
              docker.withRegistry(DOCKER_REGISTRY, registryCredential) {
                import_export.push("latest")
              }
              docker.withRegistry(DOCKER_REGISTRY, registryCredential) {
                customer.push("latest")
              }
              docker.withRegistry(DOCKER_REGISTRY, registryCredential) {
                contract.push("latest")
              }
              docker.withRegistry(DOCKER_REGISTRY, registryCredential) {
                booking.push("latest")
              }
            }
          }
        }
        stage('Preparing deploy to k8s') {
          steps {
            // input 'Deploy to Production?'
            // milestone(1)
            sshagent(credentials: ['jenkins_k8s']) {
              sh '''
              ssh -o StrictHostKeyChecking=no -l root 172.255.255.196 "export KUBECONFIG=/etc/kubernetes/admin.conf && kubectl delete -f /home/admins/backoffice-k8s/mandala-hotel-admin-deployment.yaml && kubectl delete -f /home/admins/backoffice-k8s/mandala-hotel-booking-deployment.yaml && kubectl delete -f /home/admins/backoffice-k8s/mandala-hotel-contract-deployment.yaml && kubectl delete -f /home/admins/backoffice-k8s/mandala-hotel-customer-deployment.yaml && kubectl delete -f /home/admins/backoffice-k8s/mandala-hotel-danh-muc-deployment.yaml && kubectl delete -f /home/admins/backoffice-k8s/mandala-hotel-im-export-deployment.yaml"
              '''
            }
          }
        }
        stage('Deploy to k8s') {
          steps {
            // input 'Deploy to Production?'
            milestone(1)
            sshagent(credentials: ['jenkins_k8s']) {
              sh '''
              ssh -o StrictHostKeyChecking=no -l root 172.255.255.196 "export KUBECONFIG=/etc/kubernetes/admin.conf && kubectl apply -f /home/admins/backoffice-k8s/mandala-hotel-admin-deployment.yaml && kubectl apply -f /home/admins/backoffice-k8s/mandala-hotel-booking-deployment.yaml && kubectl apply -f /home/admins/backoffice-k8s/mandala-hotel-contract-deployment.yaml && kubectl apply -f /home/admins/backoffice-k8s/mandala-hotel-customer-deployment.yaml && kubectl apply -f /home/admins/backoffice-k8s/mandala-hotel-danh-muc-deployment.yaml && kubectl apply -f /home/admins/backoffice-k8s/mandala-hotel-im-export-deployment.yaml"
              '''
            }
          }
        }
      }
    }
 ```
 Jenkinsfile với Angular
 ```
 //  Gitlab
	def gitRepository = 'https://gitlab.com/HuyNV1996/holiday-backoffice-angular.git'
	def gitBranch = 'main'
	def gitlabCredential = 'holiday-backoffice-angular'

    // Harbor
	def imageGroup = 'holiday-backoffice'
	def appName = "angular"
	def namespace = "backoffice-angular"	
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
			stage("Build Docker Image"){
    		    steps {
    		        script{
				        sh '''#!/bin/bash
        		        cd ./ClientApp
        		        yarn
        		        yarn run build:prod
        		        node --max_old_space_size=12000 ./node_modules/@angular/cli/bin/ng build --configuration production
        		        docker rmi reg.mandalaholiday.tk/holiday-backoffice/angular -f
        		        '''
        		        app = docker.build('reg.mandalaholiday.tk/holiday-backoffice/angular', './ClientApp')
                        app.inside {
                            sh 'echo Hello, World!'
                        }
        		    }
    		    }
		    }
		    stage('Push Docker Image') {
                steps {
                    script {
                        docker.withRegistry(DOCKER_REGISTRY, registryCredential) {
                            // app.push("${env.BUILD_NUMBER}")
                            app.push("latest")
                            }
                        }
                    }
                }
		    stage('Deploy to k8s') {
                steps {
                    // input 'Deploy to Production?'
                    // milestone(1)
                    sshagent(credentials: ['jenkins_k8s']) {
                      sh '''
                        ssh -o StrictHostKeyChecking=no -l root 172.255.255.196 "export KUBECONFIG=/etc/kubernetes/admin.conf && kubectl delete -f  /home/admins/backoffice-k8s/mandala-hotel-angular-deployment.yaml && kubectl apply -f  /home/admins/backoffice-k8s/mandala-hotel-angular-deployment.yaml"
                      '''
                    }
                }
            }
		}
	}
 ```
 Tạo ssh private key
```
ssh-keygen -t rsa -m PEM
 ```
