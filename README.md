# DevOpstask4

Create A dynamic Jenkins cluster and perform task-3 using the dynamic Jenkins cluster.
Steps to proceed as:

1.  Create container image that’s has Linux  and other basic configuration required to run Slave for Jenkins. ( example here we require kubectl to be configured )
2. When we launch the job it should automatically starts job on slave based on the label provided for dynamic approach.
3. Create a job chain of job1 & job2 using build pipeline plugin in Jenkins 
4.  Job1 : Pull  the Github repo automatically when some developers push repo to Github and perform the following operations as:
    1.  Create the new image dynamically for the application and copy the application code into that corresponding docker image
    2.  Push that image to the docker hub (Public repository) 
 ( Github code contain the application code and Dockerfile to create a new image )
5. Job2 ( Should be run on the dynamic slave of Jenkins configured with Kubernetes kubectl command): Launch the application on the top of Kubernetes cluster performing following operations:
    1.  If launching first time then create a deployment of the pod using the image created in the previous job. Else if deployment already exists then do rollout of the existing pod making zero downtime  for the user.
    2. If Application created first time, then Expose the application. Else don’t expose it.
    
    
 # Step 1
  Firstly we have to create a Dockerfile having web server installed.
  Pushing the html code which will be launched on this web server 
  and this file to github.  
              
                    
                    FROM centos:latest
                    RUN yum install sudo -y
                    RUN yum install /sbin/service -y
                    RUN yum install httpd -y
                    COPY *.html /var/www/html
                    CMD /usr/sbin/httpd -DFOREGROUND && /bin/bash
                    EXPOSE 80
    
    
 # Step 2
 
 Here, we will create a Jenkins job which will download 
 the code and Dockerfile from the github automatically
 on pushing it to the github.
    



# Step 3

This Job of Jenkins will build our docker image from the file.
After building the image it can be pushed to docker hub public
repository.



# Step 4

For creating the container image first we have to configure the
kubectl by creating a local yum repo of kubernetes in our redhat vm.

    cat <<EOF > /etc/yum.repos.d/kubernetes.repo
    [kubernetes]
    name=Kubernetes
    baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
    enabled=1
    gpgcheck=1
    repo_gpgcheck=1
    gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
    EOF






# Step 5
