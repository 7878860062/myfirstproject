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

![hello 81](https://user-images.githubusercontent.com/64473684/84865595-20cb5180-b096-11ea-9abb-93745d1b710f.PNG)


2. Testing:

![6](https://user-images.githubusercontent.com/64473684/84867190-6721b000-b098-11ea-8ef2-54c6b0352594.PNG)

## 2. Changed made but not approved:

1. Production:

![hello 81](https://user-images.githubusercontent.com/64473684/84865595-20cb5180-b096-11ea-9abb-93745d1b710f.PNG)

2. Testing:

![6](https://user-images.githubusercontent.com/64473684/84867190-6721b000-b098-11ea-8ef2-54c6b0352594.PNG)

3. QA rejected

## Changed and approved:

1. Testing :

![82 COLOR](https://user-images.githubusercontent.com/64473684/84867721-43ab3500-b099-11ea-9c4c-1bcf480c36f1.PNG)


2. QA Approved

![www](https://user-images.githubusercontent.com/64473684/84494062-72569380-acc6-11ea-8a0f-30415ab3319b.PNG)

![qa1](https://user-images.githubusercontent.com/64473684/84494220-bb0e4c80-acc6-11ea-80ae-fa2054fb3bf7.jpg)

3. Production

![81 COLOR](https://user-images.githubusercontent.com/64473684/84867941-8bca5780-b099-11ea-826b-ad11a8221e49.PNG)



## Steps and Configurations Screenshots :

## 1. Production:

* Created a job for cloning the Github repo's master branch which is to be deployed on docker.
Github Githook trigger is checked so that whenever the developer pushes the code, github webhook will be activated and it will    contact jenkins, then jenkins will pull the updated code, and move it to the created volume.

![w3](https://user-images.githubusercontent.com/64473684/84496803-3a9e1a80-accb-11ea-8a54-08567b986344.PNG)


* Provided the command to check wether the docker conatiner is already running, if not create it mount the volume and expose it's port 80 to base os port 8081.

![w5](https://user-images.githubusercontent.com/64473684/84497666-e005be00-accc-11ea-8db9-7fde67c92b62.PNG)

## 2. Testing:

* Created a job for cloning the Github repo's master branch which is to be deployed on docker.
Github Githook trigger is checked so that whenever the developer pushes the code, github webhook will be activated and it will contact jenkins, then jenkins will pull the updated code, and move it to the created volume.

![w9](https://user-images.githubusercontent.com/64473684/84498616-9fa73f80-acce-11ea-9a6b-40d3d0019d22.PNG)

* Provided the command to check wether the docker conatiner is already running, if not create it mount the volume and expose it's port 80 to base os port 8082 QA team will check the testing environment at this port address.

![w10](https://user-images.githubusercontent.com/64473684/84499284-f06b6800-accf-11ea-8668-1637e1ffa32c.PNG)

## 2. QA Team :

* Provided the github repo link along with credentials as on approval there should be a merge between the master and dev branch, and also the code should be pushed to github.



* Details is provided to the additional behaviour so that merging could be done.

![1c01](https://user-images.githubusercontent.com/64473684/84503924-b2bf0d00-acd8-11ea-94ad-2c9bd3d91e34.jpg)

* Post Build Action deatils provided, so that push after the merge can be done

![1c03](https://user-images.githubusercontent.com/64473684/84504145-0f222c80-acd9-11ea-8a52-e536dea5fd4f.jpg)

* After the push is being done. This job will call the job1 and job2 so that the code can be updated. (This part can be omitted as webhook will do it's job, but to be on safe side, I have put these projects as downstream projects)
![1c04](https://user-images.githubusercontent.com/64473684/84504304-54def500-acd9-11ea-8d76-9d456ea27cc8.jpg)














