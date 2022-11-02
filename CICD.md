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