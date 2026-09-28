# Cloud Deployment Guide (AWS EC2 & Docker)

A straightforward guide covering key learnings on setting up an AWS EC2 instance, configuring Elastic IPs, deploying a project using Docker, and managing application updates.

---

## 📌 Key Learnings & Concepts

1. **IAM Services**: Managing user access, permissions, and service connections.
2. **AWS EC2 (Elastic Compute Cloud)**: Launching and managing virtual machines.
3. **Elastic IP**: Securing a permanent, static public IPv4 address for your instance.
4. **Docker**: Containerizing applications to run consistently across environments.
5. **Security Groups**: Configuring network firewall rules (opening ports).

---

## 🛠️ Step 1: Setting Up the AWS EC2 Instance

1. Open the **AWS Management Console** and navigate to **EC2**.
2. Click **Instances** > **Launch Instances**.
3. Set your server details:
   - **Name**: `level5-server` (or your preferred name)
   - **OS / AMI**: Select **Ubuntu** (keep default settings for other configurations).
4. Download or select your Key Pair file (e.g., `level5-kp.pem`) and launch the instance.

### 🔗 Assigning an Elastic IP Address
1. In the left navigation menu under **Network & Security**, click **Elastic IPs**.
2. Click **Allocate Elastic IP address** and click **Allocate**.
3. Select the newly allocated Elastic IP, click **Actions** > **Associate Elastic IP address**.
4. Select your instance (`level5-server`) and click **Associate**.

### 💻 Connecting to Your EC2 Machine
1. Go back to **Instances**, select your running instance, and click **Connect**.
2. Switch to the **SSH Client** tab and copy the SSH command provided at the bottom.
3. Open your local command prompt (CMD) or terminal in the folder where your key pair file (`level5-kp.pem`) is saved.
4. Paste and execute the copied SSH command to log into your EC2 instance.

---

## 🚀 Step 2: Deploying Your Project with Docker

Once inside your EC2 terminal:

1. **Clone the repository**:
   ```bash
   git clone <your_git_repo_url>
   ```
2. **Navigate into the root directory**:
   ```bash
   cd <repository_folder_name>
   ```
3. **Install Docker** (if not already installed):
   ```bash
   sudo apt update
   sudo apt install docker.io -y
   ```
4. **Build the Docker Image**:
   ```bash
   sudo docker build -t level5 .
   ```
5. **Run the Docker Container**:
   ```bash
   sudo docker run -d -p 5000:5000 level5
   ```
   *(Note: `-d` runs it in detached mode in the background).*

### 🌐 Opening Port Access (Security Groups)
1. In the EC2 Console, go to **Network & Security** > **Security Groups**.
2. Select the Security Group associated with your EC2 instance.
3. Edit **Inbound Rules** and click **Add Rule**:
   - **Type**: Custom TCP
   - **Port Range**: `5000`
   - **Source**: `0.0.0.0/0` (Anywhere)
4. Click **Save rules**.

### ⚡ Accessing the Application
Open your browser and navigate to:
```text
http://<your-elastic-ip>:5000
```

---

## 🔄 Step 3: Updating Your Application Code

When you make changes to your local code and push them to GitHub, follow one of the methods below to update your running EC2 machine.

### Method 1: Manual Update (CLI)

1. Connect to your EC2 instance via SSH.
2. Pull the latest changes from GitHub:
   ```bash
   git pull
   ```
3. Stop the currently running container:
   ```bash
   # Find the running container ID
   sudo docker ps

   # Stop the container
   sudo docker stop <container_id>
   ```
4. Rebuild the Docker image with updated code:
   ```bash
   sudo docker build -t level5 .
   ```
5. Run the new container:
   ```bash
   sudo docker run -d -p 5000:5000 level5
   ```

---

### Method 2: Automated Update via CI/CD Pipeline *(Recommended)*

Instead of manually pulling code and restarting containers, set up a **CI/CD Pipeline** (e.g., GitHub Actions):
- Automatically triggers on every `git push`.
- Builds the new image and updates the application on the EC2 virtual machine automatically.