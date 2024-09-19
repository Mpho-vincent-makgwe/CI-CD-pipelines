---

## **CI/CD Deployment for Spring Boot Application**
**Course-end Project 1**

### **Project Objective:**
As a Full Stack Developer, the objective of this project is to build and automate a CI/CD pipeline for a Spring Boot web application. The pipeline will demonstrate continuous integration and deployment, with the application hosted on an AWS EC2 instance. The source code for the application is managed on GitHub and Jenkins is used as the automation tool for building, testing, and deploying the application.

### **Tools and Technologies:**
- **Eclipse**: IDE used for Spring Boot development.
- **GitHub**: For version control and source code management.
- **Jenkins**: For continuous integration and deployment automation.
- **AWS EC2**: For hosting the Spring Boot application.
- **Maven**: For building the Spring Boot project.
- **Java**: Java 11 for the Spring Boot application.

---

### **Step-by-Step Process**

#### **1. Setting Up the GitHub Repository**
1. Initialize a local Git repository in the Spring Boot project directory:
   ```bash
   git init
   ```
2. Add the project files to the repository:
   ```bash
   git add .
   git commit -m "Initial commit"
   ```
3. Push the code to the GitHub repository:
   ```bash
   git remote add origin https://github.com/your-username/your-repo-name.git
   git push -u origin master
   ```
4. **Create a `.gitignore` file** to exclude unnecessary or sensitive files from being pushed to GitHub:
   ```
   target/
   *.jar
   *.class
   application.properties
   ```
   - The `.gitignore` file ensures build artifacts and sensitive files are not tracked in GitHub.

#### **2. Configuring Jenkins for CI/CD Pipeline**
1. **Install Jenkins on AWS EC2**:
   - Set up Jenkins on an EC2 instance by following the installation process (ensuring Java 11 is installed on the instance).
   
2. **Create a New Job in Jenkins**:
   - In Jenkins, go to **Dashboard** > **New Item** and select **Freestyle Project**.
   - Provide a name for the project (e.g., "SpringBoot-CICD") and click **OK**.

3. **Configure Source Code Management**:
   - In the job configuration, under **Source Code Management**, select **Git**.
   - Enter the URL of your GitHub repository (e.g., `https://github.com/your-username/your-repo-name.git`).
   - If necessary, add GitHub credentials for accessing the private repository.

4. **Build Triggers**:
   - Optionally, enable **GitHub hook trigger** for automatic builds whenever code is pushed to the GitHub repository.
   
5. **Build Step**:
   - Add a **Build Step** > **Invoke top-level Maven targets**.
   - In the Maven Goals section, enter the command:
     ```
     clean install
     ```
   - This will build the project, ensuring the Spring Boot application is compiled and packaged into a JAR file.

#### **3. Deploying the Spring Boot Application to AWS EC2**
1. **Prepare the EC2 Instance for Deployment**:
   - Ensure that **Java** and other necessary dependencies are installed on the EC2 instance:
     ```bash
     sudo yum update -y
     sudo amazon-linux-extras install java-openjdk11
     ```
   
2. **SSH Configuration for Jenkins**:
   - Install the **Publish Over SSH Plugin** in Jenkins.
   - Go to **Manage Jenkins** > **Configure System** and scroll down to **Publish over SSH**.
   - Add your EC2 instance details:
     - **Hostname**: Your EC2 public IP.
     - **Username**: `ec2-user`.
     - **Private Key**: Upload the content of your PEM file for authentication.
   - Save the configuration.

3. **Post-Build Action: SSH Deployment**:
   - In the Jenkins job configuration, add a **Post-build Action** > **Send files or execute commands over SSH**.
   - Select the EC2 instance configuration.
   - Set the commands to:
     ```bash
     # Copy the JAR file to the EC2 instance
     scp -i /path-to-your-key.pem target/your-app.jar ec2-user@ec2-ip:/home/ec2-user/

     # SSH into the EC2 instance and run the Spring Boot application
     ssh -i /path-to-your-key.pem ec2-user@ec2-ip 'nohup java -jar /home/ec2-user/your-app.jar &'
     ```
   - This will transfer the compiled JAR file to the EC2 instance and run the application in the background.

4. **Enable Jenkins Job**:
   - After setting everything up, go back to the Jenkins dashboard and click **Build Now** to trigger the CI/CD pipeline.

5. **Access the Application**:
   - Once deployed, you can access the Spring Boot application via the EC2 public IP:
     ```
     http://<your-ec2-public-ip>:8080
     ```

#### **4. Documentation of the Deployment Process**
- **GitHub Repository**: Share the GitHub repository link where the source code is hosted.
- **Tracked and Ignored Files**: In the `.gitignore` file, the following files are ignored during Git push:
  ```
  target/
  *.jar
  *.class
  application.properties
  ```
  - These files include build artifacts and sensitive configuration details.

- **CI/CD Pipeline Steps**:
  1. Code changes pushed to GitHub trigger the Jenkins pipeline.
  2. Jenkins pulls the code from the repository and builds it using Maven.
  3. The packaged JAR file is transferred to the EC2 instance via SSH.
  4. The application is run on the EC2 instance and is accessible via its public IP.

---

### **Conclusion**
This project demonstrates how to set up a CI/CD pipeline for a Spring Boot application using Jenkins and AWS EC2. The pipeline automates the build and deployment processes, allowing for continuous integration and deployment. The steps documented here provide a clear guide for replicating this process, ensuring successful project completion. The GitHub repository and Jenkins job are integral to this setup, enabling automated deployment and minimizing manual intervention.

---
