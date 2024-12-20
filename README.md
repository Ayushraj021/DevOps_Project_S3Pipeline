# S3Pipeline: CI/CD with Jenkins, Ansible, S3 Integration, and Monitoring Tools

S3Pipeline demonstrates how to set up an automated Continuous Integration and Continuous Deployment (CI/CD) pipeline using Jenkins, AWS S3, Ansible, and integrates monitoring tools like **Prometheus** and **Grafana**. This project automates code building, artifact storage in AWS S3, deployment to Tomcat servers using Ansible, and monitoring through Prometheus and Grafana.

---

## Project Links

- [Scripts & Playbooks Repository](https://github.com/Ayushraj021/all-setups.git)
- [Jenkins Java Project](https://github.com/Ayushraj021/jenkins-java-project.git)
- [Prometheus & Grafana Installation Guide](https://github.com/Ayushraj021/all-setups.git) *(Automated installation handled via the script)*

---

## Prerequisites

Before setting up the **S3Pipeline**, ensure the following are in place:

- **Jenkins** installed and running
- **Ansible** installed and configured
- **AWS S3** bucket created for artifact storage
- **Tomcat** servers set up for deployment
- **GitHub** repository with code ready for deployment
- **Prometheus** and **Grafana** (installation handled by the script)

---

## Steps

### Step 1: Pushing Code from Git to GitHub

1. Clone the project repository:
    ```bash
    git clone https://github.com/Ayushraj021/jenkins-java-project.git
    ```
2. Make changes and push the code to GitHub.

---

### Step 2: Perform CI on Jenkins

1. Configure Jenkins to pull the project repository from GitHub.
2. Set up the Jenkins pipeline:
    - Build the project using Maven.
    - Run tests.
    - Package the project.

---

### Step 3: Store Artifacts in S3

1. **Integrate S3 with Jenkins**:
    - Install the **S3 Publish** plugin in Jenkins.
    - Configure the AWS S3 profile with your credentials.
    - Set up the Jenkins pipeline to upload WAR files to S3.

    Example pipeline code:
    ```groovy
    stage('s3') {
        s3Upload consoleLogLevel: 'INFO', 
                 entries: [[bucket: 'your-bucket-name', 
                            sourceFile: 'target/NETFLIX-1.2.2.war', 
                            selectedRegion: 'ap-south-1']],
                 profileName: 's3-bucket'
    }
    ```

---

### Step 4: Install Tomcat on Nodes

1. Use Ansible to install and configure Tomcat on worker nodes.
2. Make sure Tomcat is set up to accept WAR file deployments.

---

### Step 5: Deploy WAR File to Tomcat

1. Use Ansible playbook to deploy the WAR file to the Tomcat webapps directory.

    Sample **playbook.yml**:
    ```yaml
    - hosts: all
      tasks:
        - name: Copy WAR file to Tomcat webapps
          copy:
            src: /var/lib/jenkins/workspace/pipeline/target/NETFLIX-1.2.2.war
            dest: /root/tomcat/webapps
    ```

2. Trigger the Ansible playbook through Jenkins.

---

## Integrating Prometheus and Grafana for Monitoring

The **S3Pipeline** script automatically installs **Prometheus** and **Grafana**, so no additional manual installation is required. Once the installation is complete, the system will be set up to monitor Jenkins and Tomcat metrics.

### Prometheus Setup

1. **Prometheus Configuration**: The script configures Prometheus to scrape metrics from Jenkins and Tomcat.
2. **Jenkins Monitoring**: Use the **Prometheus Plugin for Jenkins** to expose Jenkins metrics, which will be scraped by Prometheus.
3. **Tomcat Monitoring**: Prometheus will collect resource usage data from Tomcat servers.

### Grafana Setup

1. **Grafana Configuration**: The script configures Grafana to connect to Prometheus as a data source.
2. **Dashboards**: Grafana dashboards will be automatically set up to visualize the Jenkins and Tomcat metrics.

---

## Conclusion

This repository sets up an end-to-end CI/CD pipeline with Jenkins, Ansible, S3 integration, and monitoring via Prometheus and Grafana. The automated process includes building, testing, storing artifacts, deploying them to Tomcat, and visualizing performance metrics.

---

## License

MIT License
