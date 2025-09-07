# SonarQube Setup for Code Quality Analysis (Amazon Linux)

SonarQube (SQ) is a powerful web-based code quality analysis tool. It helps detect bugs, code smells, vulnerabilities, and enforces coding standards through automated static analysis. It's widely used in CI/CD pipelines to maintain clean and secure codebases.

---

## 🌟 Key Features

- Supports multiple languages: **Java, C, Python**, and more
- Web-based dashboard for results and trends
- Built-in **quality rules**, custom **quality profiles** & **gates**
- Detects **vulnerabilities**, **bugs**, **code smells**, and **technical debt**
- Seamless integration with **Maven**, **Gradle**, **Jenkins**, **GitLab**, etc.

---

## ⚙️ System Requirements

- **Java 17+**
- **Amazon EC2 (t2.medium or higher recommended)**
- Default SonarQube port: **9000**

---

## 🖥️ SonarQube Components

1. **SonarQube Server**
2. **Rules Engine**
3. **Internal Database** (default: H2 for demo use)

---

## 📦 Installation Steps (Amazon Linux)

### 1. Launch Amazon EC2 Instance
- Instance type: `t2.medium`
- OS: Amazon Linux 2

### 2. Install Java 17
```bash
sudo amazon-linux-extras enable corretto17
sudo yum clean metadata
sudo yum install java-17-amazon-corretto -y
java -version
```

### 3. Create SonarQube Directory and Download
```bash
cd /opt
sudo yum install wget unzip -y
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-8.9.6.50800.zip
sudo unzip sonarqube-8.9.6.50800.zip
```

### 4. Create Dedicated User
```bash
sudo useradd sonar
sudo passwd sonar
sudo chown -R sonar:sonar /opt/sonarqube-8.9.6.50800
sudo chmod -R 777 /opt/sonarqube-8.9.6.50800
```

### 5. Start SonarQube
```bash
su - sonar
cd /opt/sonarqube-8.9.6.50800/bin/linux-x86-64
./sonar.sh start
```

- Access via: `http://<EC2_PUBLIC_IP>:9000`
- Default login:
  - Username: `admin`
  - Password: `admin` → You'll be prompted to change it

---

## 🚀 Analyzing a Maven Project

### 1. Create Project in SonarQube UI
- Go to **Projects → Create Project**
- Choose **Manually**
- Fill in:
  - **Project Key**
  - **Display Name**
- Click **Set Up**

### 2. Generate Token
- Name your token
- Click **Generate**
- Copy and save the token

### 3. Clone and Prepare Your Project
```bash
git clone <your-repo-url>
cd <your-project-folder>
```

### 4. Run SonarQube Maven Scanner
```bash
mvn clean sonar:sonar \
  -Dsonar.projectKey=<your_project_key> \
  -Dsonar.host.url=http://<EC2_PUBLIC_IP>:9000 \
  -Dsonar.login=<your_generated_token>

```

---

## ✅ Quality Analysis Results

- **PASSED**: Meets quality gate conditions (e.g., >85% rules satisfied)
- **FAILED**: Violates thresholds set in quality gate
- Java projects have **600+ built-in rules**
- You can create and assign:
  - **Custom Quality Profiles**
  - **Custom Quality Gates**

---

## 🔁 CI/CD Integration (Upcoming)

Integration with **Jenkins** and **GitLab CI/CD** will be added to automate:
- Code checkout
- Build process
- Static code analysis using SonarQube
- Quality gate enforcement before merge/deploy

Stay tuned for CI/CD automation scripts and pipeline templates!

---

## 📎 References

- [SonarQube Documentation](https://docs.sonarqube.org/)
- [Maven Integration Guide](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner-for-maven/)

---

> 🚀 **Write Clean Code, Deliver with Confidence.**
> Use SonarQube to build maintainable, high-quality software.

---

## SonarQube Scan Setup for Non-Maven Projects (Python, Node.js, etc.)

1. **Download SonarScanner CLI** → `wget <sonar-scanner-url>`  
2. **Install unzip** → `sudo yum install unzip -y`  
3. **Unzip scanner** → `unzip sonar-scanner-*.zip`  
4. **Move to /opt** → `sudo mv sonar-scanner-* /opt/sonar-scanner`  
5. **Set environment variables** → `export PATH=$PATH:/opt/sonar-scanner/bin`  
6. **Optionally set SONAR_SCANNER_HOME** → `export SONAR_SCANNER_HOME=/opt/sonar-scanner`  
7. **Reload bash** → `source ~/.bashrc`  
8. **Prepare project properties** → create `sonar-project.properties` in project root with project key, name, sources, host URL, and token  
9. **Run scan** → `sonar-scanner`  

> Works for Python, Node.js, C/C++, or any project SonarQube supports.

