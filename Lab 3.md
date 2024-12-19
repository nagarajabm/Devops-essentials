## Configuring `Jenkins Server` 

###  Objective: 
1. Setting up Jenkins and Installing necessary plugins
2. Configuring Jenkins to build Maven projects

### Task 1: Login to the Jenkins Server

1. Go to the **Web Browser** and open a new tab then enter the URL as shown:

   **http://< Jenkin's Public IP>:8080/** (It requests the **InitialAdminPassword** during the setup.)
   
2. To obtain the **InitialAdminPassword**, access the Jenkins Server by SSHing from the Jump Server, utilizing Jenkins' Public IP.
```
ssh ubuntu@xx.xx.xx.xx
```
From Jenkins execute the below command and copy the password.
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
   
3. Now, go back to Jenkin's landing page in the **Web Browser**:
   
   (Enter the Jenkins URL as shown: **http://< Jenkin's Public IP>:8080/**)

4. Under Unlock Jenkins, enter the above **initialAdminPassword** & click **Continue**.
5. Click on **Install suggested Plugins** on the Customize Jenkins page.
6. Once the plugins are installed, it gives you the page where you can create a New **Admin User**. 
7. Enter the **User Id** and **Password** followed by **Name** and **E-Mail ID** then click on **Save & Continue**.
8. In the next step, on the Instance Configuration Page, verify your **Jenkins Public IP** and **Port Number** then click on **Save and Finish**

### Task 2: Configuring Maven in Jenkins

Do the below in Jenkin's Dashboard:

1. Click on **Manage Jenkins** > **Plugins** > **Available Plugins** tab and search for **Maven**.
2. Select the "**Maven Integration**" Plugin and click **Install** (without restart).
3. Once the installation is completed, click on **Go back to the top page**.
4. On Home Page select **Manage Jenkins** > **Tool**.
5. Inside Tool Configuration, look for **Maven installations**, click **Add Maven**. 
6. Give the Name as **"Maven"**, Version-Default one (Latest), and **Save** the configuration.

### Task 3: Build

1. Create a new project for your application build by selecting **New Item** from the Jenkins homepage.
2. Enter an item name as **hello-world** and select the project as **Maven Project** and then click **OK.**
   ( You will be prompted to the configure page inside the hello-world project.)
3. Go to the "**Source Code Management**" tab, and select Source Code Management as **Git**, Now you need to provide the GitHub Repository's **Master Branch URL** and **GitHub Account Credentials**.
4. In the Credentials field, you have to click **Add** and then click on **Jenkins**.
5. Then you will be prompted to the **Jenkins Credentials Provider** page. Under Add Credentials, you can add your **GitHub Username**, **Password**, and **Description**. Then click on **Add**.
6. In the Source Code Management page, navigate to Credentials, and select your GitHub credentials.
7. Leave all other values as default, navigate to the "**Build**" tab, and in the "Goals and options" section, input "**clean package**". Save the configuration. (The 'clean package' command clears the target directory, Builds the project, and packages the resulting WAR file into the target directory)
8. Return to the Maven project "**hello-world**" and click on "**Build Now**" to initiate the build process for your application's **.war** file.( war file will be created in **/var/lib/jenkins/workspace/hello-world/target/**)

Task-2: Installing and Configuring Tomcat for Deploying our Application on Jenkins Server

Now, SSH into the Jenkins server (Make sure that you are the root user and Install the Tomcat web server)
Note: (If you are already in Jenkins Server, again SSH is not needed.)
Follow below steps:
```
sudo apt update
```
```
sudo apt install tomcat10 tomcat10-admin -y
```
```
sudo systemctl enable tomcat10
```
Now we need to navigate to server.xml to change the Tomcat port number from 8080 to 9999. (As port number 8080 is already being used by the Jenkins website)
```
sudo vi /etc/tomcat10/server.xml
```
(Optional step): If you are unable to open the file then change the permissions by using the below command.
```
sudo chmod 766 /etc/tomcat10/server.xml
```
Change 8080 to 9999
press esc & Enter ":" and copy paste below code and hit enter
g/8080/s//9999/g
Save the file using ESCAPE+:wq!

To Verify whether the Port is changed, execute the below Command.
```
cat /etc/tomcat10/server.xml
```
(Optional step): If you are unable to open the file then change the permissions by using the below command.
```
sudo chmod 766 /etc/tomcat10/server.xml
```
```
sudo vi /etc/default/tomcat10
```
Paste the path of jdk inside the file

JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
Now restart the system for the changes to take effect
```
sudo service tomcat10 restart
```
```
sudo service tomcat10 status
```
To exit, press ctrl+c

If any error is present asking for the value of JAVA_HOME
```
vi /etc/default/tomcat10
```
add the following line to the end of the file

JAVA_HOME="/usr/lib/jvm/java-17-openjdk-amd64"
Now restart the system for the changes to take effect
```
sudo service tomcat10 restart
```
```
sudo service tomcat10 status
```
Once the Tomcat service restart is successful, go to your web browser and enter Jenkins Server's Public IP address followed by 9999 port.
(Example: http://< Jenkins Public IP >:9999 or http://184.72.112.155:9999)

Now you can check the Tomcat running on port 9999 on the same machine.
We need to copy the .war file created in the previous Jenkins build from the Jenkins workspace to tomcat webapps directory to serve the web content
```
sudo cp -R /var/lib/jenkins/workspace/hello-world/target/hello-world-war-1.0.0.war /var/lib/tomcat10/webapps
```
The above command is copying a WAR (Web Application Archive) file from the Jenkins workspace to the Tomcat web apps directory. Let's break down the command:

sudo: Run the command with superuser (root) privileges, as copying files to system directories often requires elevated permissions.

cp: The copy command in Linux.

-R: Recursive option, used when copying directories. It ensures that the entire directory structure is copied.

/var/lib/jenkins/workspace/hello-world/target/hello-world-war-1.0.0.war: Source path, specifying the location of the WAR file in the Jenkins workspace.

/var/lib/tomcat10/webapps: Destination path, indicating the Tomcat webapps directory where the WAR file is being copied.

This command assumes that your Jenkins job has built a WAR file named hello-world-war-1.0.0.war in the specified workspace directory. It then copies this WAR file into the Tomcat webapps directory, allowing Tomcat to deploy and run the web application.

Once this is done, go to your browser and enter Jenkins Public IP address followed by port 9999 and path (URL: http://< Jenkins Public IP >:9999/hello-world-war-1.0.0/).
Now, you can see that Tomcat is now serving your web page
Now, Stop tomcat10 and remove it. Otherwise, it will slow down the Jenkins server.
```
sudo service tomcat10 stop
```
```
sudo apt remove tomcat10 -y
```
Summary:

* Opening the Jenkins Web Page on the browser.
* Unlock Jenkins and create an admin user.
* Install plugins, including Maven integration.
* Configure Jenkins to use a specific version of Maven.
* Create a Maven project in Jenkins for the "hello-world" application.
* Configure source code management with Git and GitHub.
* Define build goals and options.
* Build the project and verify the outcome.
* Install and configure Apache Tomcat for serving web applications.
* Deploy the built WAR file to Tomcat.


