# Setting up a JMX check in ECS EC2

This sandbox goes over creating an ECS cluster and deploying a Datadog Agent container and a Tomcat container for JMX monitoring

## Pre-reqs

Access to an AWS account (DD sandbox or personal)

## Creating an ECS cluster

### Step 1: Create an SSH key pair

Create a key pair so you can SSH into your EC2 instances following these instructions: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/create-key-pairs.html 

Download the private key file to a super secret location (or Downloads if you're basic)

### Step 2: Create a new ECS cluster

Go to the ECS clusters page and select `Create Cluster`

Under the Infrastructure tab, configure the following
1. Select the `Amazon EC2 instances` option
2. Create a New Auto Scaling Group
3. OS: Amazon Linux 2
4. EC2 instance type: t2.medium
5. Desired capacity: Minimum 2/Maximum 2
6. SSH Key pair: set to the key pair you created in Step 1

Click `Create` and you should see your cluster getting created with 2 EC2 instances

You can go to the EC2 Instances page to view your instances and their status

### Step 3: Connect to your EC2 instance

Click on one of your instances to pull up its details:
1. Go to the Security tab and click on the Security Group
2. Under the Inbound rules tab, click Edit inbound Rules
3. Add a new rule with SSH type and Source `My IP`
4. Go back to the instance page and click Connect
5. Follow the instructions under the SSH client tab to connect to your instance
(you may need to rename the .cer file to .pem and make sure to chmod 400)
6. Repeat steps 4-5 for the other instance (in a separate terminal)

We've now connected to both instances! If you run a `docker ps` in either instance, you should see just the ECS agent running
- ex:
```
[ec2-user@ip-172-31-17-77 ~]$ docker ps
CONTAINER ID   IMAGE                                     COMMAND                CREATED          STATUS                    PORTS                                                   NAMES
114bd8f769d9   amazon/amazon-ecs-agent:latest            "/agent"               5 minutes ago   Up 5 minutes (healthy)
```

## Run the Datadog Agent as a Daemon Service

Follow this doc to deploy the Datadog Agent: https://docs.datadoghq.com/containers/amazon_ecs/?tab=webui

Make sure to 
1. set your API key
2. use the latest JMX agent image - you can find an example task definition in the `jmx-agent.json` file

## Create a Tomcat task


