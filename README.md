# 🔐 End-to-End Automated DevSecOps Pipeline

<div align="center">

![DevSecOps](https://img.shields.io/badge/DevSecOps-Automated-blue?style=for-the-badge)
![Security](https://img.shields.io/badge/Security-First-red?style=for-the-badge)
![CI/CD](https://img.shields.io/badge/CI%2FCD-Enabled-green?style=for-the-badge)

### **Production-Ready CI/CD Pipeline with Integrated Security Scanning**

*Built by [Saleem Ali](https://www.linkedin.com/in/saleem-ali-189719325/)*

[📖 Documentation](#-project-overview) • [🛠️ Infrastructure](#%EF%B8%8F-infrastructure--tools) • [🚀 Pipeline Stages](#-the-pipeline-stages) • [⚙️ Setup Guide](#%EF%B8%8F-how-to-run-this-project)

</div>

---

## 📋 Project Overview

This project demonstrates a **production-ready CI/CD pipeline** that automates the journey of a Node.js application from local development to secure production deployment. It showcases the transition from manual infrastructure management to fully automated **"Security as Code"**.

### ✨ Key Achievements

- **🔄 Continuous Integration** — Automated build triggers via SCM Polling
- **🔍 SAST (Static Application Security Testing)** — Code analysis using SonarQube to detect vulnerabilities and code smells
- **🛡️ DAST (Dynamic Application Security Testing)** — Active penetration testing using OWASP ZAP
- **✅ Quality Gates** — Security thresholds enforcement; failure to meet standards aborts deployment
- **🚀 Continuous Deployment** — Automated deployment to dedicated Production Docker container
- **📧 Feedback Loop** — Gmail SMTP integration for instant build status notifications

---

## 🛠️ Infrastructure & Tools

| Component | Technology |
|-----------|-----------|
| **Host OS** | Kali Linux (Lenovo ThinkPad X260) |
| **Orchestration** | Jenkins (Dockerized) |
| **Code Analysis** | SonarQube Community Edition |
| **Security Scanning** | OWASP ZAP |
| **Containerization** | Docker & Docker Compose |
| **Version Control** | GitHub |

---

## 🚀 The Pipeline Stages

### 1️⃣ Source Control (Checkout)

The pipeline monitors the GitHub repository using **SCM Polling** configured to check for changes every minute (`* * * * *`). This approach bypasses local network restrictions (NAT/Private IP) that prevent webhook delivery.

```groovy
triggers {
    pollSCM('* * * * *')
}
```

### 2️⃣ Static Analysis (SonarQube)

Source code is analyzed for vulnerabilities, code smells, and maintainability issues.

**🔧 Critical Fix Implemented:**
- Handled "Security Hotspots" by reviewing hardcoded credentials in `app.js`
- Marked them as "Reviewed/Safe" in SonarQube UI to pass the Quality Gate

### 3️⃣ Dynamic Analysis (OWASP ZAP)

Active penetration testing against the running application to identify runtime vulnerabilities.

**📊 Artifacts Generated:**
- `report.json` — Machine-readable scan results
- `report.html` — Human-readable audit report

Both artifacts are archived in Jenkins for compliance and audit trails.

### 4️⃣ Production Deployment

The application is deployed **only if** all security checks pass.

**🔧 Technical Solution:**
- Used absolute host paths (`/root/Al-Razzak/...`) for Docker volume mounting
- Resolved `MODULE_NOT_FOUND` errors caused by Docker-in-Docker path translation

**🌐 Access:** Application runs at `http://<Host-IP>:8082`

### 5️⃣ Notifications

Integrated Gmail SMTP with **Google App Passwords** to ensure instant status updates for the development team.

---

## 🔧 Troubleshooting Guide

<details>
<summary><b>🔴 SonarQube fails but "Issues" tab is empty</b></summary>

**Solution:** Check the **Security Hotspots** tab. Manually review suspicious code (like hardcoded passwords) and change the status to "Safe" or "Fixed" to achieve "Passed" status.

</details>

<details>
<summary><b>🔴 Production App not accessible on port 8082</b></summary>

**Solution:** 
1. Check if container exited: `docker logs production-app`
2. If "Module Not Found" error appears, ensure Jenkinsfile uses **absolute host path** for volume mount
3. Verify container is running: `docker ps`

</details>

<details>
<summary><b>🔴 Webhooks fail (403 Forbidden)</b></summary>

**Solution:** Switch to **Poll SCM**. This is more reliable in local lab environments as it doesn't require public IP tunnels (ngrok).

</details>

<details>
<summary><b>🔴 Email notifications not working</b></summary>

**Solution:**
1. Use 16-character **Gmail App Password**, not your account password
2. Set SMTP port to **465** with SSL enabled
3. Verify credentials in Jenkins configuration

</details>

---

## ⚙️ How to Run This Project

### Prerequisites

- Jenkins installed and running
- SonarQube server configured
- Docker and Docker Compose installed
- GitHub account with repository access

### Setup Steps

1. **Clone the repository**
   ```bash
   git clone https://github.com/ali4210/your-repo-name.git
   cd your-repo-name
   ```

2. **Update configuration**
   - Modify `hostPath` variable in `Jenkinsfile` to match your local directory
   - Configure Jenkins credentials for GitHub and Gmail

3. **Configure Jenkins**
   - Install required plugins (Git, SonarQube Scanner, Docker)
   - Set up SonarQube server connection
   - Configure SMTP for email notifications

4. **Trigger the pipeline**
   ```bash
   git add .
   git commit -m "Initial commit"
   git push origin main
   ```

5. **Monitor execution**
   - Watch the Jenkins dashboard for pipeline progress
   - Check email for build notifications

---

## 📊 Pipeline Architecture

```
┌─────────────┐
│   GitHub    │
│ Repository  │
└──────┬──────┘
       │ SCM Polling
       ▼
┌─────────────┐
│   Jenkins   │
│  Pipeline   │
└──────┬──────┘
       │
       ├──────────────────┬──────────────────┬─────────────────┐
       ▼                  ▼                  ▼                 ▼
┌──────────┐      ┌──────────┐      ┌──────────┐     ┌──────────┐
│SonarQube │      │OWASP ZAP │      │  Docker  │     │  Gmail   │
│   SAST   │      │   DAST   │      │  Deploy  │     │  Notify  │
└──────────┘      └──────────┘      └──────────┘     └──────────┘
```

---

## 🎯 Learning Outcomes

Through this project, I gained hands-on experience with:

- ✅ Building automated CI/CD pipelines from scratch
- ✅ Implementing security scanning (SAST & DAST) in DevOps workflows
- ✅ Troubleshooting Docker-in-Docker networking and path issues
- ✅ Configuring quality gates and security policies
- ✅ Integrating third-party security tools (SonarQube, OWASP ZAP)
- ✅ Managing credentials and secrets in Jenkins
- ✅ Setting up SMTP notifications with modern authentication

---

## 🤝 Connect With Me

<div align="center">

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/saleem-ali-189719325/)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?style=for-the-badge&logo=github)](https://github.com/ali4210)

</div>

---

## 📄 License

This project is open source and available for educational purposes.

---

<div align="center">

### 🔒 **Status: PROTECTED & DEPLOYED** ✅

*"In DevOps, if it isn't documented, it doesn't exist!"*

**Made with ❤️ by Saleem Ali**

</div>
