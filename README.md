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

<img width="1920" height="1080" alt="1" src="https://github.com/user-attachments/assets/b7d048ec-700b-41aa-b1fb-6b5cef7d8049" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1446edf9-295a-4e84-ac30-60fdbb3ae093" />

---

## 🚀 Step 1: Connect to Your EC2 Instance

Use SSH to connect to your EC2 instance:

```bash
ssh -i your-key.pem ubuntu@<your-ec2-public-ip>
````
<img width="1920" height="1080" alt="2" src="https://github.com/user-attachments/assets/51f26cce-37c0-4b74-bb8c-8ec77e123796" />

---

## 🖥️ Step 2: Set the Hostname

Rename the instance for easier identification and reboot to apply the change:

```bash
sudo hostnamectl set-hostname Jenkins-Lab
sudo init 6
```
<img width="1920" height="1080" alt="4" src="https://github.com/user-attachments/assets/e64eb877-9863-4ba9-a2bc-205d318fe879" />


> The `sudo init 6` command restarts your EC2 instance.

After the reboot, reconnect to your EC2 instance using SSH.

<img width="1920" height="1080" alt="5" src="https://github.com/user-attachments/assets/ee0c0fda-2149-4e52-8d18-e5864ee0fa55" />

---

## ⚙️ Step 3: Update Packages and Install Java

Update your package list and install **OpenJDK 21**, which Jenkins requires to run:
<img width="1920" height="1080" alt="6" src="https://github.com/user-attachments/assets/2fb4e086-9d5a-4975-aefd-7255d46c9ee9" />
<img width="1920" height="1080" alt="7" src="https://github.com/user-attachments/assets/c5dac449-44be-4bb9-a4df-96490ec7d5dd" />
<img width="1920" height="1080" alt="8" src="https://github.com/user-attachments/assets/8003645b-9692-43e4-b1a4-1bda9db44140" />

```bash
sudo apt update
sudo apt install fontconfig openjdk-21-jre -y
java -version
```
<img width="1920" height="1080" alt="9" src="https://github.com/user-attachments/assets/0491e8dc-ed87-4d45-bf63-5282873d7374" />


> Run `java -version` to verify that Java has been installed successfully.
<img width="1920" height="1080" alt="10" src="https://github.com/user-attachments/assets/7da4021d-d0cf-4ef2-996a-2cb6a6f2d6c3" />

---

## 🧩 Step 4: Install Jenkins

Add the Jenkins repository, import its GPG key, and install Jenkins:
<img width="1920" height="1080" alt="11" src="https://github.com/user-attachments/assets/b5fef32a-6600-4157-9161-fcb005350973" />

```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update
sudo apt install jenkins -y
```
<img width="1920" height="1080" alt="12" src="https://github.com/user-attachments/assets/49e2cc4d-1756-4b82-8558-d122e9273169" />

---

## ▶️ Step 5: Start and Enable Jenkins

Start the Jenkins service and enable it to launch automatically at boot:

```bash
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```
<img width="1920" height="1080" alt="13" src="https://github.com/user-attachments/assets/b852dc56-9d6c-42a1-bca8-6f84f11191ef" />

<img width="1920" height="1080" alt="14" src="https://github.com/user-attachments/assets/1ec0a1b7-59b4-4faf-b3a2-5cb0b4f005e2" />

---

## 🌐 Step 6: Access Jenkins

Once Jenkins is running, open your web browser and navigate to:

```
http://<your-ec2-public-ip>:8080
```
<img width="1920" height="1080" alt="15" src="https://github.com/user-attachments/assets/343a7160-9a83-4e89-a546-e0ba850fc2bf" />

Retrieve the Jenkins administrator password with:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
<img width="1920" height="1080" alt="16" src="https://github.com/user-attachments/assets/aa319f1b-79f6-413d-9b7b-1c2635cf5385" />

<img width="1920" height="1080" alt="17" src="https://github.com/user-attachments/assets/5182d749-3876-4512-951d-5003482fac72" />

<img width="1920" height="1080" alt="18" src="https://github.com/user-attachments/assets/d6f266d6-f407-4dbc-b7eb-296d8d73b390" />

Copy the password and paste it into the Jenkins setup wizard to unlock Jenkins.

---

## ✅ Step 7: Complete the Initial Setup

1. Choose **Install suggested plugins**.
2. Wait for the plugin installation to complete.
3. Create your first **admin user** (see details below).
4. Confirm the Jenkins URL and finish the setup wizard.

<img width="1920" height="1080" alt="19" src="https://github.com/user-attachments/assets/2ef2bd64-0393-45ca-82da-67407281c26d" />

<img width="1920" height="1080" alt="20" src="https://github.com/user-attachments/assets/37925224-4964-4a55-ba28-6360cd6f3c32" />

---

## 👤 Step 8: Create a Jenkins User (Admin Account)

After Jenkins installation, you’ll be prompted to create your first user:

1. Enter your desired credentials:

   * **Username:** (e.g., `admin`)
   * **Password:** (your secure password)
   * **Full name:** (optional)
   * **Email address:** (optional)
<img width="1920" height="1080" alt="21" src="https://github.com/user-attachments/assets/60675490-d8a7-4ce7-a473-d0a6348f1b77" />

<img width="1920" height="1080" alt="22" src="https://github.com/user-attachments/assets/1780fca8-70e2-4c34-a292-7723034f2212" />

2. Click **Save and Continue**.
3. Confirm the Jenkins URL (usually `http://<your-ec2-public-ip>:8080`) and click **Save and Finish**.
<img width="1920" height="1080" alt="23" src="https://github.com/user-attachments/assets/eb26c4a6-a3bf-4d06-8f7a-6ce02daf96e1" />

<img width="1920" height="1080" alt="24" src="https://github.com/user-attachments/assets/8a71c99b-7ee3-4ce6-aacc-574f73cca2ee" />

If you need to create additional users later:

1. From the Jenkins dashboard, go to **Manage Jenkins → Users → Create User**.
2. Fill in the required details.
3. Click **Create User** to save.
<img width="1920" height="1080" alt="25" src="https://github.com/user-attachments/assets/b6fa9b62-fca1-4f9b-bf49-d7ec048fc1d1" />

<img width="1920" height="1080" alt="26" src="https://github.com/user-attachments/assets/cba63457-c43c-4329-b31a-16f6b9ae5dfc" />

<img width="1920" height="1080" alt="27" src="https://github.com/user-attachments/assets/18677f01-8bc2-486c-86ce-2f29c7c1ec2a" />

<img width="1920" height="1080" alt="28" src="https://github.com/user-attachments/assets/7c6a3989-b899-489f-b234-5e5e482c28d7" />

<img width="1920" height="1080" alt="29" src="https://github.com/user-attachments/assets/8dee87fa-654c-45a3-8e6f-789babc3479d" />

---

## 🛠️ Step 9: Create Your First Jenkins Project (Freestyle Job)

1. From the Jenkins dashboard, click **“New Item”**.
2. Enter a project name (e.g., `My-First-Project`).
3. Select **“Freestyle project”** and click **OK**.
4. Click **Save**.
5. To run the job, click **Build Now** on the project page.
6. View the console output by clicking the build number under **Build History**.

---
<img width="1920" height="1080" alt="32" src="https://github.com/user-attachments/assets/743308a5-4298-4427-ae09-c26c8f38691b" />

<img width="1920" height="1080" alt="33" src="https://github.com/user-attachments/assets/e59cf0a0-559d-4254-aaaf-54abd4e24ebd" />

<img width="1920" height="1080" alt="34" src="https://github.com/user-attachments/assets/7d080063-847c-4ebf-98b2-06acef49eb2d" />

<img width="1920" height="1080" alt="35" src="https://github.com/user-attachments/assets/0460f6d1-1538-4a90-ba21-c453b47ba089" />

<img width="1920" height="1080" alt="36" src="https://github.com/user-attachments/assets/55064eb1-66cc-404c-8086-21ecc6505d08" />

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
