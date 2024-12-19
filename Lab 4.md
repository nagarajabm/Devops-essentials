
## Lab 4: Using GitWebHook to build your code automatically using Jenkins

**Objective:**
This lab focuses on configuring Git WebHooks to automatically trigger Jenkins builds when code changes are pushed to a GitHub repository.

---------------------------------------------------------------------
### Task-1: Configure Git WebHook in Jenkins

1. Go to the Jenkins webpage and choose **Manage Jenkins** > **Plugins**
2. Go to the **Available** Tab, Search for **GitHub Integration**. Select the **GitHub Integration Plugin** and then on **Install** (without restart).
3. Once the installation is complete, click on **Go back to the top page**.
4. In your **hello-world project**, Click on **Configure**.
5. Go to **Build Triggers** and enable the **GitHub hook trigger for GITScm polling**. Then **Save**.
6. Go to your **GitHub website**, and inside the **hello-world** repository > **Settings** > **Webhooks** and Click on the **Add Webhook**.
7. Now fill in the details as below.
8. 
#### Payload URL Example: 
* http://< jenkins-PublicIP >:8080/github-webhook/ (**Example:** http://184.72.112.155:8080/github-webhook/)
* **Content type:** application/JSON

Then, Click on **Add Webhook**.

---------------------------------------------------------------------
### Task-2: Verifying whether the WebHook is working or not by editing the Source Code

1. Now, Make a minor change and commit in GitHub's **hello-world repository** by editing **hello.txt** file.
2. As the source code gets changed, Jenkins gets triggered by the WebHook and starts building the new source code.
3. Go to Jenkins, and you can see a build is happening automatically.
4. Observe the successful build on the Jenkins page.

**Summary:**
1. Configure Git WebHooks in Jenkins for automatic triggering of builds.
2. Create a GitHub WebHook for a specific GitHub repository.
3. Make a minor code change in the GitHub repository to trigger a build in Jenkins.
4. Verify that Jenkins successfully starts a new build when changes are pushed.

#### =============================END of LAB-04=============================
