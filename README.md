# Setting up a JMX check in ECS EC2

This sandbox goes over creating an ECS cluster and deploying a Datadog Agent container and a Tomcat container for JMX monitoring

## Pre-reqs

1. Access to an AWS account (DD sandbox or personal)
2. Install the AWS CLI: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html 

## Creating an ECS cluster

### Step 1: Create an SSH key pair

Create a key pair so you can SSH into your EC2 instances following these instructions: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/create-key-pairs.html 
Download the private key file to a super secret location (or Downloads if you're basic)

### Step 2: Create a new ECS cluster

Go to the ECS clusters page and select `Create Cluster`

Under the Infrastructure tab, configure the following
- Select the `Amazon EC2 instances` option
- Create a New Auto Scaling Group
- OS: Amazon Linux 2
- EC2 instance type: t2.medium
- Desired capacity: Minimum 2/Maximum 2
- SSH Key pair: set to the key pair you created in Step 1

Click `Create` and you should see your cluster getting created with 2 EC2 instances
You can go to the EC2 Instances page to view your instances and their status

### Step 3: Connect to your EC2 instances

Click on one of your instances to pull up its details:
- Go to the Security tab and click on the Security Group
- Under the Inbound rules tab, click Edit inbound Rules
- Add a new rule with SSH type and Source `My IP`
- Go back to the instance page and click Connect
- Follow the instructions under the SSH client tab to connect to your instance
(you may need to rename the .cer file to .pem and make sure to chmod 400)
