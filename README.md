# docker-jenkins-build-projects

This repo is a list of configuration methods to deploy Jenkins 
using docker-compose, kubernetes, terraform, and provision Jenkins 
for development/test cases. 
 This has not been tested for Production Usage.


## DOCKER (docker-compose)
- Setup: To spin up jenkins using docker compose, you can run the following command

```
docker-compose up -d
```

The container saves the $JENKINS_HOME and Jenkins data outside of the Jenkins container
    volumes:
      - ./jenkins:/var/jenkins_home

- Access: You can access your new jenkins instance from your browser 
by going to http://localhost:8081/ 

- Secrets: When initial setup is performed a secrets file is created. This will be used to 
create an admin account in Jenkins. 

That file can be found in the container /var/jenkins_home/secrets/initialAdminPassword 

To access the password you can run the following command in your terminal 
```
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

Or save it to a file 

```
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword > jenkinspass.txt
cat jenkinspass.txt 
```


## KUBERNETES
The files inside the kuberenetes dir can be used to apply jenkins and a jenkins storage block to kuberentes instances such as minikube or microk8s via the kubectl package.

# Setting up kind files 
 To apply and setup the deployment you can 
run the following commands in order. 

```
cd kuberenetes/
```
**All at once:**
```
kubectl apply -f jenkins-service.yaml,jenkins-deployment.yaml,jenkins-claim0-persistentvolumeclaim.yaml,jenkins-claim1-persistentvolumeclaim.yaml,jenkins-claim2-persistentvolumeclaim.yaml
```

**Or one at at time:** 
```
kubectl apply -f jenkins-service.yaml
kubectl apply -f jenkins-deployment.yaml
kubectl apply -f jenkins-claim0-persistentvolumeclaim.yaml
kubectl apply -f jenkins-claim1-persistentvolumeclaim.yaml
kubectl apply -f jenkins-claim2-persistentvolumeclaim.yaml
```

# Check Service with minikube
minikube service jenkins

kubectl describe svc jenkins


# TERRAFORM 
