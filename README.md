# Jenkins and SonarQube Installation Guide
This guide will walk you through the process of installing Jenkins and SonarQube on an AWS Ubuntu instance, and adding the SonarQube plugin to Jenkins.

## Prerequisites
- An AWS account
- An Ubuntu instance launched on AWS
- SSH access to the Ubuntu instance
## Steps
1. Update the package list:
```bash
sudo apt-get update
```
2. Install Java:
```bash
sudo apt-get install default-jdk -y
```
3. Install Jenkins:
```bash
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins -y
```
4. Start Jenkins:
```bash
sudo systemctl start jenkins
```
5. Verify that Jenkins is running by visiting `http://<your-ec2-instance-public-ip>:8080` in a web browser.
6. Install SonarQube:
```bash
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.0.1.46107.zip
unzip sonarqube-9.0.1.46107.zip
sudo mv sonarqube-9.0.1.46107 /opt/sonarqube
```
7. Configure SonarQube by editing the `sonar.properties` file:
```bash
sudo nano /opt/sonarqube/conf/sonar.properties
```
In the file, uncomment and set the following lines:
```bash
sonar.jdbc.username=<db-username>
sonar.jdbc.password=<db-password>
sonar.jdbc.url=jdbc:mysql://localhost:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance
```
8. Start SonarQube:
```bash
sudo /opt/sonarqube/bin/linux-x86-64/sonar.sh start
```
9. Verify that SonarQube is running by visiting `http://<your-ec2-instance-public-ip>:9000` in a web browser.
10. Install the SonarQube plugin in Jenkins:
- Navigate to Jenkins > Manage Jenkins > Manage Plugins.
- Select the Available tab and search for "SonarQube Scanner".
- Check the box next to "SonarQube Scanner for Jenkins" and click Install without restart.
11. Configure the SonarQube plugin:
- Navigate to Jenkins > Manage Jenkins > Configure System.
- Scroll down to the "SonarQube servers" section.
- Click "Add SonarQube" and fill in the following fields:
 - Name: SonarQube
 - Server URL: http://<your-ec2-instance-public-ip>:9000
 - Authentication token: <your-sonarqube-auth-token>
- Click Save.
## Conclusion
You have now installed Jenkins and SonarQube on an AWS Ubuntu instance, and added the SonarQube plugin to Jenkins. You can now use Jenkins to automate your build and analysis process using SonarQube.