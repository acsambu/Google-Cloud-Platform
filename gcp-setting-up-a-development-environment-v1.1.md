# LAB: App Dev: Setting up a Development Environment v1.1
## Objectives
In this lab, you will learn how to perform the following tasks:
- Provision a Google Compute Engine instance.
- Connect to the instance using SSH.
- Install software on the instance.
- Verify the software installation.

## Steps
1. Creating a Compute Engine Virtual Machine Instance
    - In the Cloud Platform Console, select the Cloud Shell icon to launch the Cloud Shell Terminal, execute the following command

            gcloud beta compute --project=qwiklabs-gcp-00-dcd51868aca1 instances create dev-instance --zone=us-central1-a --machine-type=e2-medium --subnet=default --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=821375540936-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/cloud-platform --tags=http-server --image=debian-10-buster-v20200910 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=dev-instance --no-shielded-secure-boot --no-shielded-vtpm --no-shielded-integrity-monitoring --reservation-affinity=any

    - Set a firewall rule to allow HTTP traffic by executing the following command

            gcloud compute --project=qwiklabs-gcp-00-dcd51868aca1 firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server

2. Connect to the instance using SSH.
    - In the Cloud Platform Console, on the Navigation menu, click Compute Engine
    - On the VM instances page, in the row for the dev-instance, click SSH (in the Connect column).

3. Install software on the instance.
    - In the SSH session, to update the Debian package list, execute the following command:
        
            sudo apt-get update

    - To install Git, execute the following command:
    
            sudo apt-get install git
    If prompted, press Enter.
    - To download the Node.js setup script, execute the following command:
        
            curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -

    - To install npm and Node.js, execute the following command:
    
            sudo apt install nodejs

4. Configure the VM to Run Application Software
    - To check the version of Node.js, execute the following command:
        
            node -v
    
    You should see the Node.js version number for version 6.
    - To clone the class repository, execute the following command:
        
            git clone https://github.com/GoogleCloudPlatform/training-data-analyst

    - To change the working directory, execute the following command:
        
            cd ~/training-data-analyst/courses/developingapps/nodejs/devenv/

    - To run a simple web server, execute the following command:
        
            sudo node server/app.js

    - Return to the Cloud Console VM instances list, and click on the External IP address for the dev-instance.
        
        You should see a *Hello GCP dev!* message from Node.js.

    - Return to the SSH window, and stop the application by pressing Ctrl+C.

    - To install the Node.js library for Compute Engine, execute the following command:
        
            npm install

    - To run a simple Node.js application that lists Compute Engine instances, execute the following command:

            node list-gce-instances.js
        Many details about your machine should appear in the terminal window.

    Warning: If you try to do this on your own machine, it will not work if no credentials have been set up to access GCP on your machine.

