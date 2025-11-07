

# 🚀 Node.js Deployment on Jenkins Using Freestyle Project

Automating your **Node.js application deployment** with **Jenkins** helps you deliver updates quickly, reliably, and consistently.  
This tutorial walks you through setting up **Jenkins Freestyle Projects** to automatically pull code from GitHub, install dependencies, and deploy your Node.js app using **PM2**.

---

## 🧩 Project Overview

This project demonstrates how to automate the deployment of a **Node.js application** on an **AWS EC2 instance** (or any Linux server) using **Jenkins Freestyle Jobs**.

---

## ⚙️ Prerequisites

Before starting, ensure you have the following:

- 🖥️ **Ubuntu / Linux Server**
- ☁️ **Jenkins installed and running**
- 🟢 **Node.js and npm installed**
- 🔥 **PM2 process manager installed**
- 🔑 **GitHub repository containing your Node.js project**



# ⚙️ Setting Up Jenkins Freestyle Project

We will create **four Jenkins Freestyle Jobs** to automate the deployment pipeline:


- **Job 1:** `01-setup-env` → Install Node.js, npm, and PM2 (globally)  
- **Job 2:** `02-node-pull` → Pull the latest source code from Git  
- **Job 3:** `03-install-dependency` → Install Node.js dependencies (`npm install`)  
- **Job 4:** `04-deploy-node` → Deploy/start the Node.js application using PM2  


![](./screenshot/Screenshot%202025-11-07%20111326.png)


## 🧱 Job 1: Setup Environment

1. Click **New Item**
2. Enter name: `01-setup-env`
3. Choose **Freestyle project**, then click **OK**

### Build Steps → Execute Shell:
```bash
sudo apt install nodejs -y
sudo apt install npm -y
sudo npm install -g pm2
Add Post-build Action → Build Other Projects
```
Enter downstream project name: 02-node-pull

Click Save
 ![](./screenshot/Screenshot%202025-11-07%20111146.png)

## 🧩 Job 2: Pull Repo from GitHub
1. Click **New Item**
2. Enter name: `02-node-pull`
3. Choose Freestyle project, then click OK

Scroll to Source Code Management, select Git

Enter your Git repository URL, for example:
```
https://github.com/iamtruptimane/node-js-app-CICD.git
```
Branch: main 

Add Post-build Action → Build Other Projects

Enter downstream project name: 03-install-dependency

Click Save

![](./screenshot/Screenshot%202025-11-07%20111203.png)

## ⚡ Job 3: Install Dependencies

1. Click New Item
2. Enter name: 03-install-dependency
3. Choose Freestyle project, then click OK

4. Build Steps → Execute Shell:
```
cd /var/lib/jenkins/workspace/02-node-pull
sudo npm install
```
Add Post-build Action → Build Other Projects

Enter downstream project name: 04-deploy-node

Click Save

![](./screenshot/Screenshot%202025-11-07%20111220.png)

## 🚀 Job 4: Deploy Node.js Application
1. Click New Item

2. Enter name: 04-deploy-node

3. Choose Freestyle project, then click OK

4. Build Steps → Execute Shell:
```
cd /var/lib/jenkins/workspace/02-node-pull
pm2 start app.js --name node-app || pm2 restart node-app
```

![](./screenshot/Screenshot%202025-11-07%20111239.png)

## ▶️ Running the Pipeline
1. Go to Jenkins Dashboard

2. Click on 01-setup-env

3. Click Build Now

This will trigger the downstream jobs automatically in order:

01-setup-env → Install Node.js, npm, and PM2

02-node-pull → Pull latest code from GitHub

03-install-dependency → Install dependencies

04-deploy-node → Deploy/start app using PM2

🌐 Access the Application
Open your browser and visit:
```
http://<Public-IP>:3000
```

![](./screenshot/Screenshot%202025-11-07%20111123.png)




## 🧠 Conclusion

By setting up Node.js deployment on Jenkins using Freestyle Projects, you’ve built a simple yet powerful CI/CD pipeline that automates code pull, dependency installation, and app deployment with PM2.

This approach saves time and reduces human errors — ensuring your app stays up-to-date and reliable.
While Freestyle projects are perfect for beginners, you can later migrate to Jenkins Pipeline as Code (Jenkinsfile) for better flexibility and scalability.

With automation in place, you can focus more on building features and less on manual deployment — turning ideas into running applications faster than ever!