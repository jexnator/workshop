## Hands-on with Git and Terraform (Local)

---

#### Objectives

- **Experience the Terraform workflow**: Write, Plan, Apply
- **Understand Terraform state** before and after execution
- **Gain practical experience** with Git and Terraform on Windows
- **Create a simple local infrastructure** using Terraform

---

#### Prerequisites

- **Git**
- **Terraform**
- **Visual Studio Code (VSCode)**
- *(If not installed, consider using [Chocolatey](https://chocolatey.org/) for easy installation)*

---

#### Step 1: Create 'code' Folder

- Navigate to: `C:\Users\<your-username>\`
- Create a new folder named: `code`

---

#### Step 2: Create 'hands-on-marti' Folder

- Inside the `code` folder, create a new folder named: `hands-on-marti`
- This folder will serve as your **local Terraform repository**

---

#### Step 3: Open 'hands-on-marti' in VSCode

- Launch **Visual Studio Code**
- Open the `hands-on-marti` folder:
  - Go to **File** > **Open Folder** > Select `hands-on-marti`

---

#### Step 4: Open a New Terminal in VSCode

- In VSCode, navigate to:
  - **Terminal** > **New Terminal**
- This opens a terminal at the project's root directory

---

#### Step 5: Initialize Git Repository

In the terminal, run the command:

  ```bash
  git init
  ```

This initializes a new **Git repository** in your project folder

---

#### Step 6: Create 'main.tf' File

- In VSCode, create a new file:
  - **File** > **New File** > Save as `main.tf` in `hands-on-marti` folder
- This file will contain your **Terraform configuration**

---

#### Step 7: Write Terraform Configuration

Add the following code to `main.tf`:

  ```hcl
  terraform {
    required_providers {
      filesystem = {
        source  = "sethvargo/filesystem"
        version = "1.0.0"
      }
    }
  }

  provider "filesystem" {}

  resource "filesystem_file_writer" "hello_world" {
    path     = "${path.module}/temp/hello.txt"
    contents = "Hello World!"
  }
  ```

- **Explanation**:
  - **terraform block**: Specifies required providers
  - **provider**: Configures the filesystem provider
  - **resource**: Creates a file `hello.txt` with content "Hello World!"

---

#### Step 8: Install the Filesystem Provider

Run the following commands in the terminal:

  ```powershell
  # Download the provider
  Invoke-WebRequest -Uri "https://github.com/sethvargo/terraform-provider-filesystem/releases/download/v1.0.0/terraform-provider-filesystem_1.0.0_windows_amd64.zip" -OutFile "$env:USERPROFILE\Downloads\terraform-provider-filesystem.zip"

  # Extract the provider
  Expand-Archive -Path "$env:USERPROFILE\Downloads\terraform-provider-filesystem.zip" -DestinationPath "$env:USERPROFILE\Downloads\terraform-provider-filesystem" -Force

  # Create plugins directory
  New-Item -ItemType Directory -Force -Path "$env:APPDATA\terraform.d\plugins"

  # Move the provider to the plugins directory
  Move-Item -Path "$env:USERPROFILE\Downloads\terraform-provider-filesystem" -Destination "$env:APPDATA\terraform.d\plugins\terraform-provider-filesystem" -Force
  ```

---

#### Step 9: Initialize Terraform

In the terminal, run:

  ```bash
  terraform init
  ```

**What it does**:
  - Initializes the Terraform working directory
  - Downloads necessary provider plugins
**Significance**:
  - Prepares your environment for Terraform operations

---

#### Step 10: Validate Terraform Configuration

Run:

  ```bash
  terraform validate
  ```

**Purpose**:
  - Checks the syntax and validity of your configuration files and ensures your code is error-free before proceeding

---

#### Step 11: Plan Terraform Execution

Run:

  ```bash
  terraform plan
  ```

**Purpose**:
  - Creates an execution plan
  - Shows what actions Terraform will perform
**Benefits**:
  - Allows you to verify changes before applying
  - Prevents unexpected modifications

---

#### Understanding `terraform plan`
**Dry Run**:
  - Simulates the changes without affecting real resources
**Insightful**:
  - Displays resource additions, deletions, and modifications
**Best Practice**:
  - Always review the plan before applying the changes to your real infrastructure

---

#### Step 12: Apply Terraform Configuration

Run:

  ```bash
  terraform apply
  ```

**Process**:
  - Applies the changes specified in your configuration
  - You will be prompted to confirm; type `yes` to proceed
**Outcome**:
  - Resources are created as defined

---

#### Step 13: Verify the Results

- Navigate to your project directory
- Check for the `temp` folder
- Open `hello.txt` inside `temp` folder
  - Contents should be: **"Hello World!"**

---

#### Understanding Terraform State
**State File (`terraform.tfstate`)**:
  - Records information about the infrastructure managed by Terraform
**Purpose**:
  - Keeps track of resource mappings
  - Essential for planning and applying future changes
**Insight**:
  - Changes in state reflect real-world infrastructure status

---

#### Step 14: Clean Up Resources

Run:

  ```bash
  terraform destroy
  ```

**Purpose**:
  - Destroys resources defined in your configuration
**Note**:
  - For local filesystem changes, `destroy` may not remove files
  - Useful in cloud environments to avoid residual resources

---

#### Understanding `terraform destroy`
**Functionality**:
  - Reverses the actions performed by `terraform apply`
**Best Practice**:
  - Use cautiously; irreversible in production environments

---

#### Task recap / key learnings

**Terraform Workflow**:
  - Experienced the core commands: Write, Plan, Apply
**Infrastructure as Code**:
  - Managed infrastructure declaratively using code
**Version Control with Git**:
  - Initialized a Git repository to track changes
**State Management**:
  - Understood the role of Terraform state in tracking resources
**Provider Usage**:
  - Installed and utilized a custom provider

---
