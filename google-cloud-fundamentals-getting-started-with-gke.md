# LAB: Google Cloud Fundamentals: Getting Started with GKE
## Objectives
In this lab, you learn how to perform the following tasks:
- Provision a Kubernetes cluster using Kubernetes Engine.
- Deploy and manage Docker containers using kubectl.


## Steps
1. Confirm that needed APIs are enabled
    - In GCP console, on the top right toolbar, click the Open Cloud Shell button.
    - Click Continue. 
    - Execute the following command to list all enabled services, confirm that the following are listed:
        - Kubernetes Engine API
        - Container Registry API
   
   If either API is missing, enable the APIs and Services by executing the following commands:
            
            gcloud services enable container.googleapis.com

            gcloud services enable containerregistry.googleapis.com

2. Start a Kubernetes Engine cluster
    - For convenience, place the zone that Qwiklabs assigned you to into an environment variable called MY_ZONE. At the Cloud Shell prompt, type this partial command:
        
            export MY_ZONE=

        followed by the zone that Qwiklabs assigned to you. Your complete command will look similar to this:

            export MY_ZONE=us-central1-a

    - Start a Kubernetes cluster managed by Kubernetes Engine. Name the cluster webfrontend and configure it to run 2 nodes:

            gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2
        It takes several minutes to create a cluster as Kubernetes Engine provisions virtual machines for you.
    - After the cluster is created, check your installed version of Kubernetes using the kubectl version command:

            kubectl version
        The gcloud container clusters create command automatically authenticated kubectl for you.

    - View your running nodes in the GCP Console by executing the following command
            
            gcloud compute instances list

        Your Kubernetes cluster is now ready for use.



3. Run and deploy a container
    - From your Cloud Shell prompt, launch a single instance of the nginx container. (Nginx is a popular web server.)
        
            kubectl create deploy nginx --image=nginx:1.17.10
        In Kubernetes, all containers run in pods. This use of the kubectl create command caused Kubernetes to create a deployment consisting of a single pod containing the nginx container. A Kubernetes deployment keeps a given number of pods up and running even in the event of failures among the nodes on which they run. In this command, you launched the default number of pods, which is 1.

        **Note**: If you see any deprecation warning about future version you can simply ignore it for now and can proceed further.

    - View the pod running the nginx container:
    
            kubectl get services
        You can use the displayed external IP address to test and contact the nginx container remotely

        It may take a few seconds before the External-IP field is populated for your service. This is normal. Just re-run the kubectl get services command every few seconds until the field is populated.


    - Open a new web browser tab and paste your cluster's external IP address into the address bar. The default home page of the Nginx browser is displayed.
    - Scale up the number of pods running on your service:
        
            kubectl scale deployment nginx --replicas 3
        
        Scaling up a deployment is useful when you want to increase available resources for an application that is becoming more popular.

    - Confirm that Kubernetes has updated the number of pods:
    
            kubectl get pods

    - Confirm that your external IP address has not changed:

            kubectl get services

        Return to the web browser tab in which you viewed your cluster's external IP address. Refresh the page to confirm that the nginx web server is still responding.

