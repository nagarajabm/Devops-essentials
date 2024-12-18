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


