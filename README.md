# CloudwatchAgent-StressTest
Installing and Configuring the CloudWatch Agent, Conducting EC2 Stress Tests, and Setting Up Amazon SNS Notifications

### 1. Introduction:

This project focuses on deploying the CloudWatch agent on an Amazon EC2 instance running Linux. By doing so, you'll be able to collect and send system metrics, logs, and custom data to Amazon CloudWatch for monitoring and analysis. We will also do a little stress test on the EC2 Instance, in order to spike up the CPU utilization and ultimately configure SNS notifications.

###2. Launch an EC2 Instance

a. Launch an EC2 instance:

   - Choose an appropriate Amazon Machine Image (AMI) with a Linux operating system (e.g., Amazon Linux 2).
   - Select an instance type (e.g., t2.micro).
   -Ensure you launch a Linux or Linux 2 machine as they have the CW agent package pre-installed

### 3. Connect to the EC2 Instance

a. Connect to your EC2 instance:
  
   - Use SSH or any other remote access method to connect to the instance.
   - In this project we are connecting the instance via EC2 Instance Connect

### 4. Install the CloudWatch Agent

a. Install the CloudWatch agent package:
   
   Once you are connected to the instance type the following command:

   sudo yum install -y amazon-cloudwatch-agent
   
   - This command installs the CloudWatch agent on your EC2 instance.

c. Verify the installation:
   
   sudo systemctl status amazon-cloudwatch-agent
   
   - Ensure that the agent is active.

In order for the CW agent to send metrics to CloudWatch we need to attach the appropriate IAM Role to the EC2 instance

### 5. Create an IAM Role for CloudWatch

a. Navigate to the IAM console:

   - In the AWS Management Console, go to Services > IAM.

b. Choose "Roles" from the navigation pane on the left.

c. Create a new role:

   - Click 'Create role'.
   - Select 'AWS service' as the trusted entity type.
   - Choose 'EC2' as the common use case.
   - Click 'Next'.

d. Select the CloudWatchAgentServerPolicy:

   - In the list of policies, select the checkbox next to 'CloudWatchAgentServerPolicy'.
   - Click 'Next'.

e. Review and create the role:

   - Provide a meaningful name for the role (e.g., "CloudWatchAgentRole").
   - Click 'Create role'.

### 6. Attach the IAM Role to Your EC2 Instance

a. Associate the IAM Role you created with your EC2 instance:

   - Go to your EC2 instance details.
   - Click Actions > Instance Settings > Attach/Replace IAM Role.
   - Select the role you created and click 'Apply'.

### 7 .Set Up CloudWatch Alarm:

   -Navigate to the CloudWatch service.
   -Click “Alarms” in the left navigation pane.
   -Click “Create alarm.”
   -Choose the EC2 instance you want to monitor.
   -Select “CPU utilization” as the metric.
   -Set the threshold type to “static.”
   -Configure the threshold value (e.g., 15%).
   -Choose “Greater” for the condition.
   -Click “Next.”
   -For the alarm state trigger, select “In alarm.”
   -Create a new SNS topic (e.g., “15_percent_threshold”) and provide an email address for notifications.
   -Confirm the subscription when you receive an email from AWS.
   -Click “Create topic.”
   -Give a name to the alarm (e.g., “High CPU Utilization Alarm”).
   -Click “Create alarm” to set up the CloudWatch alarm1.
   -Install Stress Test Tool on EC2 Instance:
   -Connect to the EC2 instance via Instance Connect.
   -Run the following command to install the stress test tool:
    
    sudo yum install stress -y

### 8. Run the Test

a. Once the installation is complete, run the stress test with the following command:

sudo stress --cpu 8 --timeout 600 &

   -This will run the test for 600 seconds, simulating high CPU load.

b. Monitor CloudWatch Dashboard:
   
   -Go to the CloudWatch Dashboard.
   -Select the EC2 instance you want to monitor.
   -Observe that the CPU utilization has skyrocketed due to the stress test.

The alarm you created will trigger, and you’ll receive an email notification from AWS indicating that the threshold was crossed.

Congratulations! You’ve successfully created an alarm using the stress tool to monitor CPU utilization on your EC2 instance. 🎉🚀

