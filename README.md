# CICD-pipeline--using-jenkins-nexus-sonarqube-slack-and-selenium
Continuous integration &amp; Continuous delivery using jenkins,nexus,sonarqube,slack and selenium

Problem:
Agile methodology involves delivering small pieces of code daily. So, If the code deployment process is delayed until the development is completed, then this can cause a serious problem. There will be unnecessary rework & delay at the end of the project.

Solution:
To test the code whenever it is committed.
Automating the process of fetching the code from git, running unit tests, performing code analysis and notifying the developers.
By this technique, bugs can be identified quickly and can be fixed at an early stage, avoiding rework at the end.
This makes, the agile methodology more manageable.

Design:

![image](https://user-images.githubusercontent.com/102613218/229340608-1c516057-7acd-44a5-99ba-5b11a75a86ce.png)

Steps involved:

Continuous Integration:

Step1: Build Job - Fetch the code from GitHub repository and perform mvn install, specify the format in which the war file needs to be generated.

Step2: Unit testing job - Perform mvn unit test.

Step3: Integration testing job - Use mvn verify -DskipUniTests to run integrated test cases.

Step4: Perform code analysis - initial code analysis is performed using checkstyle.

Step5: Quality gates using sonarqube - perform second set of analysis by configuring jenkins with sonarqube plugins and quality gates.
Setup the necessary properties and post build action based on the result of the quality gate.

Step6: Deploy artifact to nexus - Once the continuous integration steps are completed and the build is successful, then the final step is to store the built artifact from step1 to a nexus repository.
Specify how artifact name should be formed and each version needs to be stored.

All these steps are integrated with slack, so concerned people are notified via slack on build success, failure.

Continuous Delivery:

Step7: Upload artifact to staging environment running on tomcat. Copy the artifact from nexus repository and upload the artifact to tomcat.

Step8: Test URL job to check if the web application is up and running. This is done by using HTTP plugin of jenkins.

Step9: Selenium testing is performed using a slave node running on windows. Install the necessary plugins and perform selenium testing.

Step10: Finally, the artifact is deployed to production server.

EC2 Instances:
Nexus
Sonarqube
Staging server
Production server
Jenkins server
Backend server
Windows server to run selenium.

![image](https://user-images.githubusercontent.com/102613218/229342675-09f14d80-4751-46b8-9a19-dc41166b06fd.png)


