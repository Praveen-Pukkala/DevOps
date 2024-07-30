# Project Description: Automate Infrastructure Provisioning with Terraform

## Description:
Infrastructure automation is critical for disaster recovery, testing, and development. As your organization adopts DevOps, there is a need to set up a centralized Jenkins server using Terraform to automate infrastructure provisioning. Terraform will be used to provision infrastructure components, and Ansible to manage configurations and deploy applications.

### Tools Required:
- Terraform
- AWS account with security credentials
- Keypair

## Expected Deliverables:
1. Launch an EC2 instance using Terraform.
2. Connect to the instance.
3. Install Jenkins, Java, and Python in the instance.

## Implementation:

### Step 1: Install Terraform and Set Up AWS Credentials

#### Create IAM Role and Access Keys:
1. Log in to the AWS Management Console and navigate to the IAM service.
2. Click on "Roles" and then "Create Role".
3. Choose "AWS service" as the trusted entity and select "EC2".
4. Attach the "AmazonEC2FullAccess" policy.
5. Review and create the role.
6. Navigate to "Users" and click "Add User".
7. Provide a name and select "Programmatic access".
8. Attach the IAM role created earlier.
9. Review and create the user.
10. Copy the access key and secret key to a secure location.

#### Configure AWS CLI:
- Ensure AWS CLI is installed. Follow [installation instructions](https://aws.amazon.com/cli/).

### Step 2: Write Terraform Script to Launch EC2 Instance
1. Create a directory for your Terraform project.
2. Inside the directory, create a file named `main.tf`.
3. Write the following code in `main.tf`:

    ```hcl
    provider "aws" {
      access_key = "YOUR_ACCESS_KEY"
      secret_key = "YOUR_SECRET_KEY"
      region     = "us-east-1"
    }

    resource "aws_instance" "project_instance" {
      ami           = "ami-007855ac798b5175e"
      instance_type = "t2.micro"
      key_name      = "terraformkeypair"

      tags = {
        Name = "Terraform-automated-instance"
      }
    }
    ```

### Step 3: Run Terraform Script to Launch EC2 Instance
1. Open a terminal and navigate to your Terraform project directory.
2. Initialize the Terraform working directory:
    ```sh
    terraform init
    ```
3. Validate the Terraform configuration:
    ```sh
    terraform validate
    ```
4. Apply the Terraform configuration:
    ```sh
    terraform apply
    ```
5. Confirm the changes by typing "yes".

### Step 4: Connect to EC2 Instance Using SSH
1. Note the public IP address of the instance from the Terraform output.
2. Navigate to the directory containing your AWS key pair.
3. Set the key pair file permissions:
    ```sh
    chmod 400 YOUR_KEY_PAIR_NAME.pem
    ```
4. Connect to the EC2 instance:
    ```sh
    ssh -i YOUR_KEY_PAIR_NAME.pem ubuntu@PUBLIC_IP_ADDRESS
    ```

### Step 5: Install Jenkins, Java, and Python on EC2 Instance
1. Update the package list and install Java and Python:
    ```sh
    sudo apt-get update
    sudo apt-get install openjdk-8-jdk python3 python3-pip
    ```
2. Install Jenkins:
    ```sh
    curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
    echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
    sudo apt-get update
    sudo apt-get install jenkins
    ```
3. Start the Jenkins service:
    ```sh
    sudo systemctl start jenkins
    ```
4. Open Jenkins web interface at `http://PUBLIC_IP_ADDRESS:8080`.
5. Retrieve the Jenkins unlock key:
    ```sh
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```
6. Complete the Jenkins setup wizard.

You now have an EC2 instance running with Jenkins, Java, and Python installed, ready for automation tasks.
