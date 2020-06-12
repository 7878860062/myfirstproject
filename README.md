![1](https://user-images.githubusercontent.com/64473684/84465195-022f1a00-ac94-11ea-87eb-d5d331c7afa2.jpg)
# Objective :

Setup up an infrastructure such as there are three teams/environemnts :

1. Production
2. Testing
3. Quality Assurance

Production Team will deploy the code first, now there is some work going on in the developement team which is to be deployed on testing environment which will be sent to the production only when the QA team approves it.

## Requirements :

1. Docker
   - Image: httpd
2. Jenkins
   - Github Plugin
3. System with 2gb ram (Recommended)
4. Git and Hooks
5. Github and Github Webhooks
6. ngrok

# My Approach

1. Made 3 Jobs for produnction, testing and Quality Assurance

2. Make Environments for production and testing. I am using docker here, I have created a volume(persistant storage) for production and testing and mounted them to their respective containers

## Process :

### Operations Side:

1. My job 1 and job2 will get their code from the github (from their respective branches => master:production, dev:testing)
2. Both the containers have diferent web addresses so the quality team can monitor both
3. Quality team continously checks for changes in testing environemnt, and decides wether to approves it or not
4. On approval QA team manually builds the job3
5. Since job three is the upstream job of job2 and job1 , so on succesfull approval foloowong things will happen:
   1. Dev and Master branch will be merged
   2. Code will be pushed to the github
   3. job 1 and job2 will be build

- ngrok will be running in the system to provide public accessable addres so that github webhook will work and production site can be accessible to the public

### Developer site :

1. Created a hook for post-commit and post-merge so the developer need not to be manually push the code each time he commits.
![1](https://user-images.githubusercontent.com/64473684/84466652-7d45ff80-ac97-11ea-89e0-1d1bb16e612d.PNG)
![2](https://user-images.githubusercontent.com/64473684/84472691-4971d680-aca5-11ea-8aeb-0d44d6210ebe.PNG)
2. Added The webhooks to the Jenkins Github API :=> ip/github-webhook/
![webhooks](https://user-images.githubusercontent.com/64473684/84473373-94d8b480-aca6-11ea-82bf-ecb0e698d38c.jpg)

## Screenshots :

## 1. Initially:

1. Production:

![a1](https://user-images.githubusercontent.com/64473684/84481887-6b268a00-acb4-11ea-9aa3-d9d1e1dd917a.PNG)

2. Testing:

![a2](https://user-images.githubusercontent.com/64473684/84482017-a4f79080-acb4-11ea-9e7f-81d93e586208.PNG)

## 2. Changed made but not approved:

1. Production:

![a1](https://user-images.githubusercontent.com/64473684/84481887-6b268a00-acb4-11ea-9aa3-d9d1e1dd917a.PNG)


2. Testing:

![a2](https://user-images.githubusercontent.com/64473684/84482017-a4f79080-acb4-11ea-9e7f-81d93e586208.PNG)

3. QA rejected

## Changed and approved:

1. Testing :

![a4](https://user-images.githubusercontent.com/64473684/84485365-b000ef80-acb9-11ea-88b5-332a990fb9aa.PNG)

2. QA Approved

![www](https://user-images.githubusercontent.com/64473684/84494062-72569380-acc6-11ea-8a0f-30415ab3319b.PNG)

![qa1](https://user-images.githubusercontent.com/64473684/84494220-bb0e4c80-acc6-11ea-80ae-fa2054fb3bf7.jpg)

3. Production

![a4](https://user-images.githubusercontent.com/64473684/84495581-03c70500-acc9-11ea-8dfc-17a3220ca93a.PNG)


## Steps and Configurations Screenshots :

## 1. Production:

* Created a job for cloning the Github repo's master branch which is to be deployed on docker.
Github Githook trigger is checked so that whenever the developer pushes the code, github webhook will be activated and it will    contact jenkins, then jenkins will pull the updated code, and move it to the created volume.

![w3](https://user-images.githubusercontent.com/64473684/84496803-3a9e1a80-accb-11ea-8a54-08567b986344.PNG)


* Provided the command to check wether the docker conatiner is already running, if not create it mount the volume and expose it's port 80 to base os port 8084.

![w5](https://user-images.githubusercontent.com/64473684/84497666-e005be00-accc-11ea-8db9-7fde67c92b62.PNG









