## Prerequisites for the Ubuntu Server
Need to install the java version 17\
Have the sonarqube latest version (sonarqube-10.4.0.87286) and jenkins (latest) installed on same server but with different user accounts

Faced the issue, showing error that looks like this
![Screenshot (439)](https://github.com/Danishbansal-cmd/Jenkins-Zero-To-Hero/assets/67588499/0778ee14-4227-4cc3-b0d1-f3b0ec5f1452)

![Screenshot (444)](https://github.com/Danishbansal-cmd/Jenkins-Zero-To-Hero/assets/67588499/05204891-34dd-4edc-ad4d-43fae29f5165)

To solve this, I just need to downgrade the sonarqube version from the server by installing this version

```wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.4.0.54424.zip```

and starting the "sonarqube-9.4.0.54424.zip/bin/linux-x86-64/sonar.sh console" file
Then, the above listed error is solved, though I could not figure out the meaning of the above listed error.

## Other Error
Trying to install the minikube in the Windows, but getting the error\
then found the [Youtube Video](https://www.youtube.com/watch?v=TAM-DLPX9XA) which described that one needs to install the "minikube-windows-amd64.exe" file 
from the [Github](https://github.com/kubernetes/minikube/releases) and put into the "C:/.kube" folder with the "kubectl.exe" file and rename it to "minikube.exe"
It does solve the error and I was able to start the ```minikube start``` successfully\
One can check the status of the pods if they are running or not by writing the command ```kubectl get pods``` in terminal
