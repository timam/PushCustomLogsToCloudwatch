# PushCustomLogsToCloudwatch

This repository demonestrates how you can Push your Custom Application log to aws CloudWatch.

To achive this goal, We will need the following things

    >   Create IAM Policy
    >   Create a Role
    >   Attach the Role to EC2


### Create IAM Policy

From your aws console go to IAM, then select Policies and click on Create Policy button. 

    i.  Select CloudWatch  Logs as service

    ii. Attach the following actions to the policy 

        - CreateLogGroup
        - CreateLogStream
        - PutLogEvents
        - DescribeLogStreams
    
    iii. As resources, you may want to restrict, but I don't. I am sececting 
        - All resources 

    iv. Review the policy and hit Create Policy, Don't forget to give a name and description, I would like to name it as
        - CWlogPolicy
 
### Create a Role
From your aws console go to IAM, then select Roles and click on Create Role button. 

    i.  From AWS Services, Select 
        - EC2
    ii. From use Cases, Select 
        - Allows EC2 instances to call AWS services on your behalf. 
        And proceed by clicking Next: Permissions
    iii.Search for the policy you created in previous step
        - CWlogPolicy
        Select that and click Next: Review
    iv. Give it a nice name and discription and click Create role, I am naming it as 
        - CWlogsRole
    
### Create an Programetic User

    - Create User
    - Attach CWlogPolicy 
    - Save Access Key and Secret Key


### Attach the Role to EC2
Now we need to attach CWlogsRole to EC2 Instance. It is as simple as. 

    i.  From EC2 Console, Select the EC2 instence 
    ii. Go to actions, Select Instence Settings, Select         Attach/Replace IAM Role
    iii.Select CWlogsRole and Apply

_Now that EC2 instence will have permission to write logs to CloudWatch. The next step is to install CloudWatch Agent in EC2_

### Install CloudWatch Agent in EC2
Make sure you have installed Python3 and Python3 Pip installed in your instance. (Installation virtualenv might require)

- Download the script 
```sh
$ curl https://s3.amazonaws.com/aws-cloudwatch/downloads/latest/awslogs-agent-setup.py -O
```

- Execute the script as sudo user, Be specific about regoin. 
```sh
$ sudo python3 ./awslogs-agent-setup.py --region eu-west-1
```

------------------------------------------------------------------------------------------------------------

Launching interactive setup of CloudWatch Logs agent ... 

Step 1 of 5: Installing pip ...libyaml-dev does not exist in system DONE

Step 2 of 5: Downloading the latest CloudWatch Logs agent bits ... DONE

Step 3 of 5: Configuring AWS CLI ... 
AWS Access Key ID [****************TUNQ]: 
AWS Secret Access Key [****************mqZY]: 
Default region name [eu-west-1]: 
Default output format [None]: 

Step 4 of 5: Configuring the CloudWatch Logs Agent ... 
Path of log file to upload [/var/log/syslog]: /var/log/snort
Destination Log Group name [/var/log/snort]: 

Choose Log Stream name:
  1. Use EC2 instance id.
  2. Use hostname.
  3. Custom.
Enter choice [1]: 1

Choose Log Event timestamp format:
  1. %b %d %H:%M:%S    (Dec 31 23:59:59)
  2. %d/%b/%Y:%H:%M:%S (10/Oct/2000:13:55:36)
  3. %Y-%m-%d %H:%M:%S (2008-09-08 11:52:54)
  4. Custom
Enter choice [1]: 3

Choose initial position of upload:
  1. From start of file.
  2. From end of file.
Enter choice [1]: 1
More log files to configure? [Y]: N

Step 5 of 5: Setting up agent as a daemon ...DONE



- Configuration file successfully saved at: /var/awslogs/etc/awslogs.conf
- You can begin accessing new log events after a few moments at https://console.aws.amazon.com/cloudwatch/home?region=eu-west-1#logs:
- You can use 'sudo service awslogs start|stop|status|restart' to control the daemon.
- To see diagnostic information for the CloudWatch Logs Agent, see /var/log/awslogs.log
- You can rerun interactive setup using 'sudo python ./awslogs-agent-setup.py --region eu-west-1 --only-generate-config'



------------------------------------------------------------------------------------------------------------