# DevOps to Automate the Deployment of Web Pages with Prometheus

## Objectives
- Serve a static website on a Kubernetes cluster.
- The container that provides the website must be continuously monitored.
- For each commit in the website's repository, automatically a new image is built and pushed to DockerHub, then a container with this latest image should be deployed replacing the container with the old website on the cluster.



## Implementation

A CentOS machine with Jenkins and Ansible configures a Kubernetes Cluster to run the latest version of the website in a Nginx container monitored by Prometheus. The Bitnami's Nginx Helm Chart was used, so we can make use of its exporter (needed for Prometheus).

<p align="center">
  <img src="https://img.shields.io/badge/Jenkins-D24939?style=for-the-badge&logo=Jenkins&logoColor=white"/>  
  <img src="https://img.shields.io/badge/Ansible-000000?style=for-the-badge&logo=ansible&logoColor=white"/>
  <img src="https://img.shields.io/badge/kubernetes-326ce5.svg?&style=for-the-badge&logo=kubernetes&logoColor=white"/>
  <img src="https://img.shields.io/badge/Nginx-009639?style=for-the-badge&logo=nginx&logoColor=white"/>
  <img src="https://img.shields.io/badge/Prometheus-000000?style=for-the-badge&logo=prometheus&labelColor=000000" />

</p>

Obs: It was necessary to create a task in the playbook to delete the old nginx helm chart because the force option (force reinstall) presented some issues.


## Usage
- Jenkins needs to be configured to access: 
1) Your GitHub Repository
2) Your Docker Hub
3) The Kubernetes Cluster Machine
- After the first commit to the repository, it's necessary to do a manual Scan on Jenkins so it can obtain the configuration files and perform the build. Subsequent commits will be automatically recognized by Jenkins.

## Result
<p align="center">
  <img src="images/page.png"/>
</p>
<p align="center">
  <img src="images/login.png"/>
</p>
<p align="center">
  <img src="images/prometheus1.png"/>
</p>
<p align="center">
  <img src="images/prometheus2.png"/>
</p>
<p align="center">
  <img src="images/prometheus3.png"/>
</p>


- The static website is available at:
https://onepagelove.com/fusion-lite