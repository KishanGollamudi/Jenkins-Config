# Jenkins Installation on AWS EC2 (t2.micro)

This guide provides a step-by-step walkthrough to install and configure **Jenkins** on an **AWS EC2 t2.micro** instance running **Ubuntu**.  
It also includes steps on how to create your first Jenkins **user** and **project**.

---

## 🧰 Prerequisites

Before you begin, ensure you have:
- An **AWS EC2** instance (t2.micro or larger) running **Ubuntu**
- **SSH** access to the instance
- `sudo` privileges
- Port **8080** open in the **Security Group** (for Jenkins web access)

---

## 🚀 Step 1: Connect to Your EC2 Instance

Use SSH to connect to your EC2 instance:

```bash
ssh -i your-key.pem ubuntu@<your-ec2-public-ip>
````

---

## 🖥️ Step 2: Set the Hostname

Rename the instance for easier identification and reboot to apply the change:

```bash
sudo hostnamectl set-hostname Jenkins-Lab
sudo init 6
```

> The `sudo init 6` command restarts your EC2 instance.

After the reboot, reconnect to your EC2 instance using SSH.

---

## ⚙️ Step 3: Update Packages and Install Java

Update your package list and install **OpenJDK 21**, which Jenkins requires to run:

```bash
sudo apt update
sudo apt install fontconfig openjdk-21-jre -y
java -version
```

> Run `java -version` to verify that Java has been installed successfully.

---

## 🧩 Step 4: Install Jenkins

Add the Jenkins repository, import its GPG key, and install Jenkins:

```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update
sudo apt install jenkins -y
```

---

## ▶️ Step 5: Start and Enable Jenkins

Start the Jenkins service and enable it to launch automatically at boot:

```bash
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```

---

## 🌐 Step 6: Access Jenkins

Once Jenkins is running, open your web browser and navigate to:

```
http://<your-ec2-public-ip>:8080
```

Retrieve the Jenkins administrator password with:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy the password and paste it into the Jenkins setup wizard to unlock Jenkins.

---

## ✅ Step 7: Complete the Initial Setup

1. Choose **Install suggested plugins**.
2. Wait for the plugin installation to complete.
3. Create your first **admin user** (see details below).
4. Confirm the Jenkins URL and finish the setup wizard.

---

## 👤 Step 8: Create a Jenkins User (Admin Account)

After Jenkins installation, you’ll be prompted to create your first user:

1. Enter your desired credentials:

   * **Username:** (e.g., `admin`)
   * **Password:** (your secure password)
   * **Full name:** (optional)
   * **Email address:** (optional)
2. Click **Save and Continue**.
3. Confirm the Jenkins URL (usually `http://<your-ec2-public-ip>:8080`) and click **Save and Finish**.

If you need to create additional users later:

1. From the Jenkins dashboard, go to **Manage Jenkins → Users → Create User**.
2. Fill in the required details.
3. Click **Create User** to save.

---

## 🛠️ Step 9: Create Your First Jenkins Project (Freestyle Job)

1. From the Jenkins dashboard, click **“New Item”**.
2. Enter a project name (e.g., `My-First-Project`).
3. Select **“Freestyle project”** and click **OK**.
4. In the project configuration page:

   * **Description:** Add a short description.
   * **Source Code Management:**
     Choose **Git** and enter your repository URL (if applicable).
   * **Build Triggers:**
     Check options like “Build periodically” or “Poll SCM” if needed.
   * **Build Section:**
     Click **Add build step → Execute shell** and enter your build command (example below):

     ```bash
     echo "Hello from Jenkins!"
     ```
5. Click **Save**.
6. To run the job, click **Build Now** on the project page.
7. View the console output by clicking the build number under **Build History**.

---

## 🧾 Notes

* **Instance Type:** t2.micro
* **Default Jenkins Port:** 8080
* **Service Commands:**

  * Start Jenkins: `sudo systemctl start jenkins`
  * Stop Jenkins: `sudo systemctl stop jenkins`
  * Restart Jenkins: `sudo systemctl restart jenkins`
  * Check Status: `sudo systemctl status jenkins`

Ensure your EC2 **Security Group** allows inbound traffic on **port 8080** to access the Jenkins web interface.

---

## 🧑‍💻 Author

Created by *Kishan Gollamudi*
Date: *27th October 2025*

---

```

---

Would you like me to add one more section on **how to enable Jenkins to run on system startup automatically** and **set up firewall rules** (for Ubuntu UFW)? Those are often useful additions for production-like setups.
```
