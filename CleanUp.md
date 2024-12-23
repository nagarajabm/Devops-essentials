## Clean Up

Login to the JumpServer and navigate to the below directory
```
cd ~ && cd devops-labs
```
Execute the below command to terminate you Docker Server, Jenkins Server and delete the Key that was crated on AWS
```
terraform destroy -auto-approve
```
![image](https://github.com/user-attachments/assets/3ff871a7-f8d5-4431-a1ff-5d02d79e382a)

Now exit from the Jump Server
```
exit
```
Now navigate to your AWS Managment Console and manually terminate your JumpServer.
