---
title: "Create EC2 & Verify Private Environment"
date: 2026-07-08
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---


### Overview

In this workshop, you will learn how to deploy EC2 instances securely inside private subnets within an Amazon VPC. We will set up two EC2 instances named **rookwork-ec2-1** and **rookwork-ec2-2** within the private subnets of your custom VPC. You will configure their network settings to ensure they remain isolated from direct public internet access, and verify the environment configuration (local IP, internet route via NAT Gateway, and installation of key dependencies such as Docker, Node.js, and Git).

---

### Step 1: Launch rookwork-ec2-1

1. Open the AWS Management Console and navigate to the **EC2 Dashboard**.
2. Click on **Launch instances**.
3. Under **Name and tags**, enter `rookwork-ec2-1` in the Name field.
4. Under **Application and OS Images (Amazon Machine Image)**, select **Amazon Linux 2023 AMI** (or the AMI required for your project).

![Launch instance - Step 1](/images/5-Workshop/5.4/ec2-1-launch.png)

---

### Step 2: Configure Network Settings

1. Scroll down to the **Network settings** section and click **Edit** (top-right of the box).
2. **VPC**: Select your custom VPC (e.g., `Rookwork-VPC`).
3. **Subnet**: Select a **Private Subnet** (e.g., `rookwork-private-subnet-1`).
4. **Auto-assign public IP**: Select **Disable** (this guarantees the instance does not receive a public IP address and remains private).
5. **Firewall (security groups)**: Select or create a security group configured to allow inbound traffic only from the Bastion host, Application Load Balancer, or specific internal VPC ranges.

![Edit Network Settings](/images/5-Workshop/5.4/ec2-1-step2.png)

![Edit Network Settings](/images/5-Workshop/5.4/ec2-1-step3.png)

---

### Step 3: Key Pair and Storage

1. Under **Key pair (login)**, select your existing key pair or create a new one to securely connect to the instance later.
2. Under **Configure storage**, verify the size and type of the root volume (e.g., gp3).
3. Click **Launch instance** on the right panel to provision the instance.

---

### Step 4: Launch rookwork-ec2-2

1. Follow the same procedure as above to launch the second EC2 instance.
2. In the **Name** field, enter `rookwork-ec2-2`.
3. In **Network settings**, choose the same VPC but select the **second Private Subnet** (e.g., `rookwork-private-subnet-2`) for high availability across different Availability Zones.
4. Keep **Auto-assign public IP** as **Disable**.
5. Assign the same security group or a corresponding private security group.
6. Launch the instance.

![Launch rookwork-ec2-2](/images/5-Workshop/5.4/ec2-2-step1.png)

---

### Step 5: Connect and Verify the Private Environment

Since both EC2 instances are in private subnets, they do not have public IP addresses and cannot be accessed directly via SSH from the public internet. We will connect to the instance using **AWS Systems Manager (SSM) Session Manager**.

1. In the EC2 instances list, select the instance you want to connect to (e.g., `rookwork-ec2-2`) and click the **Connect** button at the top right.

![Connect to EC2 - Step 1](/images/5-Workshop/5.4/check-connect-ec2-step1.png)

2. Switch to the **Session Manager** tab and click the orange **Connect** button at the bottom right to establish a terminal session.

![Connect to EC2 - Step 2](/images/5-Workshop/5.4/check-connect-ec2-step2.png)

---

### Step 6: Verify Configurations and Environment

Once the terminal session is successfully established, execute the following verifications:

#### 1. Verify Internet Connectivity (via NAT Gateway)
To ensure the route table for private subnets redirects outbound traffic to a NAT Gateway (enabling packages to be updated and library downloads):
```bash
ping -c 3 google.com
```
![Environment Check Completed successfully](/images/5-Workshop/5.4/check-connect-completed.png)

#### 2. Verify Installed Software / Dependencies
Run version check commands to ensure the required runtimes and tools for the Rookwork application are installed and ready:

* **Docker**:
  ```bash
  docker --version
  ```
  *If not installed, run the following commands to install:*
  ```bash
  sudo dnf update -y
  sudo dnf install -y docker
  sudo systemctl start docker
  sudo systemctl enable docker
  sudo usermod -aG docker $USER
  ```

* **Node.js**:
  ```bash
  node --version
  ```
  *If not installed, run the following commands to install via NVM:*
  ```bash
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
  source ~/.bashrc
  nvm install 20
  ```

* **Git**:
  ```bash
  git --version
  ```
  *If not installed, run the following command to install:*
  ```bash
  sudo dnf install -y git
  ```