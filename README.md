# AWSProject_DevOps_1    -- If you want the steps to see go with code
 An Organization is using an EC2 Instance, S3 Bucket, Lambda Function, and IAM for users. Our goal is that every day at a certain time, we need to report the usage of these resources in the project to our manager.

We will write a shell script to execute this task. The shell script can be integrated with a CronJob. So, the Linux project will wait for the scheduled time and automatically execute the script. This task can be accomplished in several ways, however here I have used Shell Scripting and AWS CLI for the same.

Step-1: Create a EC2 Instance in aws console
Step-2: sudo apt-get update
Step-3: sudo snap install aws-cli --classic
Step-4: Check if it is installed or not by using (aws cli --version)
Step-5: Check and copy access key and Secret key in aws console right side click on profile icon and go for security credentials then you can see the access key and secret access key.
Step-6: Then in terminal enter (aws configure)
Step-7: Create a file using vim command
        vim aws_Resource_tracker.sh
        This will opens the editor window and in that write the bash script.
Step-8: Write the bash script in that editor window using (esc-> i) this will goes to insert mode
#!/bin/bash
#
################################################
#Author: Karthik
#Date: 06th-June
#
#Version: v1
#
#This script will report the AWS Resources usage
######################
set -x
#AWS S3
#AWS EC2
#AWS Lambda
#AWS IAM Users
#
# list S3 buckets
echo "Print list of S3 buckets"
aws s3 ls
#
# list ec2 instances
echo "Print list of ec2 instances"
aws ec2 describe-instances | jq '.Reservations[].Instances[].InstanceId'
#aws ec2 describe-instances
#
# list IAM Users
echo "Print list of IAM Users"
aws iam list-users
###################################################
Step-9: esc -> :wq! (This will helps to save the file)
step-10: Then give the permissions to the bash file (aws_Resource_tracker.sh)
         chmod777 aws_Resource_tracker.sh
Step-11: For output (./aws_Resource_tracker.sh)
Step-12: Output appears
Step-13: To see more, then use pipeline command
./aws_resource_usage.sh | more
Step-14: To find only the instance id:
aws ec2 describe-instances | jq '.Reservations[].Instances[].InstanceId'
By using this command you can get output only the instance id which are available in your aws account.


Steps to integrate a CRONJOB to the bash script.
       -> To Integrate a CRONJOB to this Bash Script
       -> pwd
       -> ls
       -> copy the path of the script.sh
       -> Or by using the command "realpath myscript.sh"
       -> chmod +x /home/ubuntu/aws_resource_usage.sh
       ->crontab -e
       -> To schedule your script (Run the script everyday at 6pm)
       -> 0 6 * * * /bin/bash /home/ubuntu/aws_resource_usage.sh >> /home/ubuntu/aws_resource_usage.log 2>&1
       -> Save and exit
       -> ctrl + o to save
       -> ctrl + x to exit
       -> To verify it was added or not
       -> crontab -l
       -> Checkk if it runs successfully or not
       -> grep CRON /var/log/syslog


