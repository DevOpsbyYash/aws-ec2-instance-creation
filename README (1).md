# Create Your First Amazon EC2 Instance (Linux)

## Introduction

In this practical, I learned how to create and launch a Linux EC2
instance using the AWS Management Console.

Amazon EC2 (Elastic Compute Cloud) is an AWS service that provides
virtual servers in the cloud. We can use these servers to host websites,
run applications, install software, and perform different computing
tasks.

EC2 allows us to create servers whenever required without purchasing or
maintaining physical hardware.

## Project Objectives

-   Understand the basics of Amazon EC2.
-   Create and launch a Linux EC2 instance.
-   Learn about AMIs and instance types.
-   Create and use an SSH key pair.
-   Configure a Security Group for SSH access.
-   Connect to a Linux EC2 instance using SSH.
-   Understand different EC2 instance states.
-   Learn how to terminate an EC2 instance.

## Prerequisites

-   Basic understanding of AWS Cloud.
-   An active AWS account.
-   Basic knowledge of Linux commands.
-   Basic understanding of SSH and key pairs.

## AWS Services and Concepts Used

  Service / Concept   Purpose
  ------------------- ---------------------------------------------------
  Amazon EC2          Creates a virtual Linux server
  Amazon Linux AMI    Provides the operating system for the instance
  Instance Type       Defines CPU, memory, and networking capacity
  Key Pair            Supports SSH authentication
  Security Group      Controls network traffic to and from the instance
  Amazon EBS          Provides block storage for the EC2 instance
  SSH                 Connects securely to the remote Linux server

## Step 1: Log in to the AWS Console

First, I logged in to the AWS Management Console using my AWS account
credentials.

I selected the AWS Region where I wanted to launch my EC2 instance.

## Step 2: Open the EC2 Dashboard

1.  Searched for **EC2** in the AWS Management Console.
2.  Opened the EC2 service.
3.  Explored the EC2 Dashboard to understand the available resources and
    options.
4.  Clicked **Launch instance** to start creating a new instance.

## Step 3: Configure the EC2 Instance

### 1. Name and Tags

I entered a name for my EC2 instance.

-   **Instance Name:** `My-Linux-EC2`

Tags are key-value pairs that help identify and organize AWS resources.

### 2. Select an Amazon Machine Image (AMI)

I selected **Amazon Linux** from the Quick Start options.

An AMI (Amazon Machine Image) is a template used to launch an EC2
instance. It includes the operating system and other required
configuration.

### 3. Select an Instance Type

I selected:

-   **Instance Type:** `t2.micro`

The instance type determines the CPU, memory, and networking capacity
available to the virtual server.

### 4. Create a Key Pair

I created a new key pair with the following configuration:

-   **Key Pair Name:** `keypair`
-   **Key Pair Type:** RSA
-   **Private Key File Format:** `.pem`

After clicking **Create key pair**, AWS downloaded the `keypair.pem`
file to my local computer.

The private key is used to authenticate when connecting to the Linux EC2
instance through SSH.

**Important:** Keep the private key secure. Never upload your `.pem`
private key file to GitHub or share it publicly.

### 5. Configure Network Settings

I configured the Security Group to allow SSH traffic.

-   **Type:** SSH
-   **Protocol:** TCP
-   **Port:** 22
-   **Source:** My IP

The **My IP** option adds my current public IP address as the allowed
source for SSH traffic. This restricts SSH access to my current network
instead of allowing connections from every IP address.

**Important:** The EC2 instance's Public IPv4 address is different from
my own public IP address. The EC2 Public IPv4 address is used as the
destination in the SSH command. If my own public IP changes, I may need
to update the Security Group rule.

### 6. Configure Storage

I kept the default storage configuration:

-   **Volume Type:** General Purpose SSD (`gp3`)
-   **Storage Size:** 8 GiB
-   **Volume:** Root volume

Amazon EBS (Elastic Block Store) provides block storage for EC2
instances.

### 7. Review and Launch

After completing the configurations, I reviewed the instance settings
and clicked **Launch instance**.

AWS then started creating my Linux EC2 instance.

## Step 4: Verify the EC2 Instance

After launching the instance, I opened the EC2 **Instances** page and
checked the following information:

-   Instance ID
-   Instance Name
-   Instance Type
-   Availability Zone
-   Public IPv4 Address
-   Private IPv4 Address
-   Instance State

### EC2 Instance States

  -----------------------------------------------------------------------
  State                               Meaning
  ----------------------------------- -----------------------------------
  Pending                             The instance is being launched

  Running                             The instance is running

  Stopping                            The instance is shutting down

  Stopped                             The instance is not running, but it
                                      can usually be started again

  Shutting-down                       The instance is being terminated

  Terminated                          The instance has been permanently
                                      terminated
  -----------------------------------------------------------------------

Once the instance reached the **Running** state and passed its status
checks, it was ready for connection.

## Step 5: Connect to EC2 Using SSH

SSH (Secure Shell) is a network protocol used to securely connect to and
manage remote servers.

For a Linux EC2 instance, an SSH client can be used to connect remotely.

### 1. Locate the Private Key

I located the downloaded `keypair.pem` file on my local computer.

### 2. Set Key Permissions (Linux / macOS)

Before connecting, restrict access to the private key:

``` bash
chmod 400 keypair.pem
```

This gives the file owner read permission and prevents other local users
from reading the key.

### 3. Connect to the EC2 Instance

Use the following SSH command:

``` bash
ssh -i keypair.pem ec2-user@<EC2-PUBLIC-IP>
```

Replace `<EC2-PUBLIC-IP>` with the **Public IPv4 address of your EC2
instance**.

For Amazon Linux, the default username is:

``` bash
ec2-user
```

Other common default usernames include:

-   Ubuntu: `ubuntu`
-   Debian: `admin`
-   Red Hat: `ec2-user`

The SSH command uses two different pieces of information:

-   `ec2-user@<EC2-PUBLIC-IP>` identifies the remote EC2 instance and
    its login user.
-   The Security Group's SSH rule allows traffic from **my current
    public IP address**.

### 4. Connect from Windows

Windows includes an OpenSSH client on many current installations. In
PowerShell, use:

``` powershell
ssh -i "C:\path\to\keypair.pem" ec2-user@<EC2-PUBLIC-IP>
```

Replace the key path and EC2 Public IPv4 address with your actual
values. If the key permissions are too open, Windows may require you to
restrict access to the key file using its file permissions.

## Step 6: Connect Using EC2 Instance Connect

AWS also provides EC2 Instance Connect for browser-based access when the
instance, operating system, and network configuration support it.

Steps:

1.  Open the EC2 Dashboard.
2.  Select the running EC2 instance.
3.  Click **Connect**.
4.  Select the **EC2 Instance Connect** tab.
5.  Click **Connect**.

## Step 7: Terminate the EC2 Instance

After completing the practical, I terminated the EC2 instance to avoid
unnecessary running-instance charges.

Steps:

1.  Open the EC2 Dashboard.
2.  Select **Instances**.
3.  Select the EC2 instance.
4.  Open **Instance state**.
5.  Select **Terminate instance**.
6.  Review the confirmation message.
7.  Confirm the termination.

The instance state changes through:

`Running → Shutting-down → Terminated`

**Important:** Termination is generally permanent. The root EBS volume
is deleted if its `DeleteOnTermination` setting is enabled. Other
attached EBS volumes may remain after termination and can continue to
incur charges. Always check your storage settings before terminating an
instance.

## What I Learned

Through this practical, I learned:

-   How to launch a Linux EC2 instance using AWS.
-   How AMIs are used to create virtual machines.
-   How instance types define computing resources.
-   How to create and use SSH key pairs.
-   How Security Groups control network access.
-   The difference between my public IP address and the EC2 Public IPv4
    address.
-   How to connect to a remote Linux server using SSH.
-   How to check EC2 instance states.
-   How to terminate an EC2 instance.

## Conclusion

This was my first hands-on practical with Amazon EC2. I learned how to
create a Linux virtual server, configure its basic networking and
storage settings, connect to it using SSH, and terminate it after use.

This practical helped me understand the basics of AWS EC2 and gave me a
foundation for working with other AWS services.

------------------------------------------------------------------------

**Author:** Yash Jain\
**Learning Path:** AWS Cloud \| Linux \| DevOps
