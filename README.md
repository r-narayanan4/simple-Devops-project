# Simple DevOps Project

---

## Introduction

This project is a simple DevOps project that aims to automate the continuous integration and deployment of a web application. Jenkins tool is used for this project.

## Prerequisites

- Git
- Jenkins
- Shell script
- Terraform
- AWS EC2

## Setup

  1. Create Tomcat and Jenkins server on AWS EC2 using `Terraform` tool.
       See this [Link](https://github.com/r-narayanan4/Jenkins-and-tomcat-setup.git)

  2. Login to jenkins server

        - Create a new job and fill the following details,
          - Source Code Management:
            - Repository: [Link](https://github.com/r-narayanan4/java-app-hello-world.git)
            - Branches to buils: `*/master`

          - Build Triggers
            - Poll SCM
            - Schedule : `*/2 * * * *` (for every two minutes)

          - Build
            - Root POM: `pom.xml`
            - Goals and options: `clean install package`

  3. Adding Deployment steps

      - we need to install a `deploy to container` plugin. This plugin is used to deploy on Tomcat server which we are using.

          - Install Maven Plugin without restart
             > `- Manage Jenkins > Manage Plugins > Available > deploy to container`

            - Setup credentials
             > - `Manage Jenkins > Manage credentials > (global) > Add credentials`
                 - Username: `deployer`
                 - Password: `********`
                 - id: `Tomcat_user`
                 - Description: `Tomcat user to deploy on tomcat server`

            - Modify the job which is created for maven project `(2.)`
              - Post-build Action
                - Deploy war/ear to container
                  - WAR/EAR files: `**/*.war`
                  - Containers: `Tomcat version`
                    - Credentials: `Tomcat_user`
                    - Tomcat URL: `<http://<Public_IP:<port_NO>`>

            - Save and build the job.

## Conclusion

This Devops Project implements the **Continuous integration** and **Continuous Deployment** for every two minutes. By Automating the deployment process, the web application will change accordingly.
