## `Git` and `GitHub` Operations.

### Objective: 
List of tasks we will be performing in this Lab
1. Initialize a local Git repository on JumpServer EC2 instance.
2. Configure Git settings like email and username.
3. Add and commit changes to the Git repository.
4. Create and switch between Git branches.
5. Make changes and commits in different branches.
6. Merge branches and Push the code to a GitHub repository.

### Pre-requisites:
1. Create a **GitHub Account** & **Empty Public Repository** with name as **"hello-world"**

   **Note:** (To SignUp for GitHub Account, [Click Here](https://github.com/signup?ref_cta=Sign+up&ref_loc=header+logged+out&ref_page=%2F&source=header-home))
2. You need to generate a **Personal Access Token** (PAT) in GitHub.
   
   <details>
     <summary>Click Here To Generate the Token:</summary>
     
   * First, Go to your `GitHub homepage,` Click on the top right `profile Icon` and then `settings`
   * Click on `Developer settings` (At the bottom on the left side menu). Click on `Personal Access Token` and then Click on `Generate New Token.`
   * Under '**Select Scopes**' select all items. Click on '**generate token**'.
   * Once generated, **Copy and save the token in a safe location as it is not visible again in GitHub.**
   
   
   </details>

3. Basic familiarity with **Git Commands.**

   Once the `GitHub Account` and `Empty repository` is ready, let's operate in the Jump Server.


### Task 1: Initializing the local git repository and committing changes

On the `Jump Server,` do the below:
```
cd ~
```
Check the GIT version (Git comes pre-installed on certain Linux machines by default). 
```
git --version
```
**(Optional)** If it does not exist, then you can install it using the below command. (Else no need to execute the below line):  
```
sudo apt install git -y
```
Download the Java Code that we are going to use in the CICD pipeline.
```
wget https://hsbctraining.s3.us-east-2.amazonaws.com/hello-world-master.zip
```
```
unzip hello-world-master.zip
```
```
ls
rm hello-world-master.zip
```
```
cd hello-world-master
ls
```
```
git init .
```
To set the `User Identity` ie... `email and user name.` you can execute below commands:

```
git config --global user.email youremail
```
```
git config --global user.name yourname
```
```
git status
```
Move the code from `Working Directory` to `Staging Area (Index)`
```
git add .
```
```
git status
```
```
git log
```
Move the code from `Staging Area (Index)` to `Local Repository`
```
git commit -m "This is the first commit"
```
```
git log
```
```
git status
```
Currently, our Code is in Local Repository.


### Task 2: Pushing the Code to your Remote GitHub Repository  

1. Create `Alias` as `new-repo` to GitHub's Remote repository URL.
```
git remote add new-repo <Replace your Repository URL> 
```
(**Example:** `git remote add new-repo https://github.com/komal/hello-world.git`)

2. To view a specific alias use the below command.
```
git remote show new-repo
```
3. Now you can push your code from `Local Repository` to Remote Repository using the below command.
```
git push new-repo master 
```
**Note:** When it asks for a password, enter the **Personal Access Token** and Press Enter

   (When you paste, PAT is invisible and It's the expected behavior.)


### Task 3: Creating a Git Branch and Pushing Code to the Remote Repository
* To create a new Branch
```
git branch dev
```
* To know the current branch
```
git branch
```
* To switch to another branch
```
git checkout dev
```
```
git branch
```
Creating a new file with content in current branch
```
vi index.html
```
change to `INSERT mode` and add the below content
```
<html>
<body>
<h1> Hi There! This file is added in dev branch </h1>
</body>
</html>
```
Save the file using `ESCAPE+:wq!`
```
git status
```
Move the file from `Working Directory` to `Staging Area (Index)`
```
git add index.html
```
```
git status
```
Move the file from `Staging Area (Index)` to `Local Repository`
```
git commit -m "adding new file index.html in new branch dev"
```
```
git log
```
Pushing the same file from `Local Repository` to Remote Repository
```
git push new-repo dev
```
```
git branch
```
```
git branch prod
```
```
git checkout prod
```
```
git branch
```
```
git merge dev
```
```
git push new-repo prod
```
```
git checkout master
```
```
git merge prod
```
```
git push new-repo master
```


