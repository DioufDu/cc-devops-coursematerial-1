#Prerequisites

## 1. Programming
Basics in JAVA, Maven, Ruby are beneficial

## 2. SAP Github Account
- Create a Github account on the SAP internal Github if you don't already have one. Use your d/i-user as ID and an *own password* (not your SAP domain password). For help see: [Getting started with git and github at SAP](https://github.wdf.sap.corp/).

## 3. Git installation & SSH keys
Perform this steps only if you do **not have** SSH keys already. If you have some, you can continue with step 4.
- Open this [link](https://git-scm.com/download/)
- Download and install git
- Follow the guide [here](https://help.github.com/enterprise/2.4/user/articles/generating-ssh-keys/) to generate SSH keys (Hint: Don't miss to select your operating system :-) ). **Remark**: In step 5 please use `github.wdf.sap.corp` as `hostname`


## 4. Create Trial Space on Cloud Foundry @ HCP
- Create your own trial space on the Cloud Foundry system using the [self-service](https://help.cf.sap.hana.ondemand.com/frameset.htm?b8ee7894fe0b4df5b78f61dd1ac178ee.html#loiob8ee7894fe0b4df5b78f61dd1ac178ee). Perform the first step only.
- You can optionally join the [CF users mailing list](https://listserv.sap.corp/mailman/listinfo/cf.users) for questions, answers and discussions

## 5. Prepare Dev Environment
Please follow the [installations steps](https://github.wdf.sap.corp/agile-se/vagrant-development-box/blob/master/VMImage_GettingStarted.md) to prepare your VM image that provides the whole development environment.

#### Jenkins
- In Terminal execute the following commands:
```
cd ~/apps/jenkins
java -jar jenkins.war
```
- Keep the terminal alive
- Jenkins should be available at [http://localhost:8080](http://localhost:8080)
  - Note that Jenkins needs a short while to start
  - You should be able to access Jenkins using this URL from the browser (Chromium) inside the VM, but also from your host system - this is because the VM is also configured for port forwarding on 8080
  - After you verified that Jenkins is running, you can quit the process (CTRL+C) and close the Terminal window

## 6. Prepare for CI-connect/ xmake Exercises
Please follow the instructions to [Add SAPNetCa Certificate to Java Keystore](Add_SAPNetCa_To_Keystore.md)

## 7. [Optional] Install Google Chrome & Postman on your local machine
- Install Google Chrome
- Install Chrome App `Postman`
