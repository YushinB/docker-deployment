# A Basic First example: Standalone Nodejs App.

![image](https://user-images.githubusercontent.com/34083808/184832399-847e0ca2-94f7-47ce-bec0-f9ef9950460a.png)

## Hosting providers.
![image](https://user-images.githubusercontent.com/34083808/184832635-5f377d2a-cfee-402d-bc02-0eff80637538.png)

## deploy to AWS EC2
![image](https://user-images.githubusercontent.com/34083808/184833854-98e977d1-2303-44fb-a620-c53efabd3673.png)

## create AWS EC2 instance. 
go to EC2 instance dashboard, click on Launch instances.<br>

![image](https://user-images.githubusercontent.com/34083808/184842955-7f71e4a3-b866-445a-b514-f3280842684c.png)

then select amazon linux version of instance. <br>
![image](https://user-images.githubusercontent.com/34083808/184843236-15d74caf-74c3-49e8-8dc8-bd10e21e922d.png)

select instance type t2.micro free tier eligible (for free). Please also select the key pair, if you don't have you can create new key pair. 
note that, this key pair only provide onece, if you lost it you have to delete your instance. we have two type of key pair one for openSSH (linux, mac os), one for putty (window). <br>

![image](https://user-images.githubusercontent.com/34083808/184843392-ca07e128-65c5-43e7-8981-17f763107f2b.png)

after completed the setting select launch instance and start connect to your virtual host. in my case I use putty. you can get the connect information in the instance connect session. <br>
![image](https://user-images.githubusercontent.com/34083808/184844387-740b2bed-58cd-464c-894e-b4f92de70bd8.png)

Putty setup for new ssh session.

![image](https://user-images.githubusercontent.com/34083808/184844801-dd9de423-07ff-44f5-abfb-b9197f4b946e.png)

![image](https://user-images.githubusercontent.com/34083808/184844950-2f48e754-9a3c-4c18-9cec-8441f064db5f.png)

after conected you'll see as below image. <br>
![image](https://user-images.githubusercontent.com/34083808/184845258-fdf7d491-beec-425a-8455-523c9c994d02.png)

please run update pakage command ``` sudo yam update -y``` for aws-linux or ```sudo apt update``` for ubuntu. 
then use can use the amazon-linux-extras to install docker, this is customize tool of amazon for easy intall program. 
``` sudo amazon-linux-extras install docker ```

to start docker service please use ``` sudo service docker start```

> incase of linux general os please folow the instructor on [docker](https://docs.docker.com/engine/install/)

## Deloy Source code vs image. <br>

![image](https://user-images.githubusercontent.com/34083808/184858580-94dbe3bb-7a5c-4f1a-a6f4-f43c309be754.png)

1. Create repository on docker hub.

![image](https://user-images.githubusercontent.com/34083808/184859482-9588c547-be04-4514-a846-1de3848de494.png)

2. Build immage on local machine and push it to Docker hub.

```
docker build -t  node-app-dep-1:1.0 .

# make a tag 
docker tag node-app-dep-1:1.0 yushinb/node-app-dep-1:1.0
# push to docker hub
# please login at first
docker login
docker push yushinb/node-app-dep-1:1.0
```

> To get access to the web page we need to set the Security groups inside EC2. 
you'll see currenly, outbound group are open for any port and any type of trafic from instance to outside, that the reason why you can download the image.

![image](https://user-images.githubusercontent.com/34083808/184916263-8bacc36d-2d01-4858-acc8-8ed7100bb7e3.png)

On the other hand, the inbound just allow some port can be access from outside the instance. 

![image](https://user-images.githubusercontent.com/34083808/184916782-029bc179-cc8b-41fd-81cb-5d2e5b07a64d.png)

# managing and updating the Container.

if you make any change in your source code then you need to build the image again. after that push image to dockerhub. 
```
docker build -t  yushinb/node-app-dep-1:1.0 

docker login
docker push yushinb/node-app-dep-1:1.0
```

update new image on virtual host machine. 
```
sudo docker pull yushinb/node-app-dep-1:1.0
sudo docker stop node-app
sudo docker run -d --rm --name node-app -p 80:80 yushinb/node-app-dep-1:1.0
```
