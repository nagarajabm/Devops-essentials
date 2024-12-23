## Configuring `Docker Machine` as Jenkins Slave, build and deploy code in Docker Host as a container

## Task-0: Configuring Key pair.
SSH into docker-server
Change to the root user
```
sudo su
```
Generate a key-pair
```
ssh-keygen
```
Add the public key to the authorized_keys
```
cat /root/.ssh/id_ed25519.pub
```
Copy the contents of the file and append in authorized_keys
```
vi /root/.ssh/authorized_keys
```
Save the file using ESCAPE+:wq!


### Task-1: Configuring Docker Machine as Jenkins Slave.

1. In Jenkins Webpage, Navigate to **Jenkin's Dashboard** and click on the **Manage Jenkins** and **Nodes**.
2. Click on **New Node** in the next window. Give the node name as `docker-slave` and Select `Permanent Agent`
3. Fill out the details for the node docker-slave as given below.
   * The name should be given as **docker-slave**
   * Remote Root Directory as **/home/ubuntu**
   * labels to be **Slave-Nodes**
   * usage to be given as **"use this node as much as possible"**
   * Launch method to be set as **"launch agents via SSH"**.
   * In the host section, give the **Public IP of the Docker instance**.
   * For Credentials for this Docker node, click on the dropdown button named **Add** and then click on **Jenkins**
   * Then in the next window, in kind select **SSH username with private key** (Give username as `root`)
   * In **Private Key** Select **Enter directly**
   
   **Note:** To get the `Private Key` go to `Docker-server` and Execute the below command:
      ```
      cd /root/.ssh
      cat id_ed25519
      ```
     (Copy the entire content of the Private Key, including the **First and Last line** till `5 hyphens` only.)
     
   * Once Copied, Paste it into the space provided for the **private key** then click on **Add**..
   * Now, In SSH Credentials, choose the newly created **Ubuntu** credentials.
   * Host Key Verification Strategy Select as **Non Verifying Verification Strategy** and **Save** it and it's done.

**Note:** Check whether the Slave Node is online/Offline.

   <details>
     <summary>(Optional Step): If still the slave node is offline, do as below:</summary>
     
   1. From Jenkin's server using Docker's Public IP SSH into Docker server.
   2. Now, Re-check whether the Slave node is Online/Offline.
   </details>

### Task-2: Build and deploy code in Docker Host on the container.

Once the Slave Node is Online, then continue the below.

1. Navigate to your **Jenkins Home page**, choose the **hello-world project** from the drop-down, and click on the Configure tab.
2. In the **General Tab**, choose **Restrict where this project can be run** and set the Label Expression to **Slave-Nodes.**
3. Move to the **Post Build Steps Tab**, select **"Run only if the build succeeds,"** and then select `add a post-build step`, choose **Execute shell** from the drop-down, paste the provided commands (below) into the shell, and click **Save.**

```
#!/bin/bash

# Navigate to the home directory
cd ~

# Create Dockerfile
cat <<EOL > Dockerfile
# Pull Base Image
FROM tomcat:8-jre8

# Maintainer
MAINTAINER "CloudThat"

# Copy the war file to the image's tomcat path
ADD hello-world-war-1.0.0.war /usr/local/tomcat/webapps/
EOL

# Copy the WAR file to the current directory
cp -f /home/ubuntu/workspace/hello-world/target/hello-world-war-1.0.0.war .

# Remove the old container if it exists
sudo docker container rm -f helloworld-container

# Build a new Docker image
sudo docker build -t helloworld-image .

# Run the new container
sudo docker run -d -p 8080:8080 --name helloworld-container helloworld-image

```

### Task 4: Building the **hello-world project**

Manually click on **"Build Now"** in Jenkins. After a successful build, access the application.

In your browser, type **"http:// < Your Docker Host Public IP >:8080/hello-world-war-1.0.0/"** to view the website.


![image](https://github.com/user-attachments/assets/59b751b0-b120-46d3-b0bd-6bb15dec8108)






