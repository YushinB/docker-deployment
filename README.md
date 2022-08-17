# docker-deployment

# Why we need docker ?
![image](https://user-images.githubusercontent.com/34083808/184829729-f3909e4f-7bfb-4239-ae78-e99eb943b784.png)

![image](https://user-images.githubusercontent.com/34083808/184829571-bb47af75-b76b-413b-820b-5968d34129b1.png)

# Development to Production: Things To Watch Out For

![image](https://user-images.githubusercontent.com/34083808/184829936-42944eea-13e8-464c-bbbf-cc25d0cfb04c.png)

# Bind Mounts, Volumes & COPY
![image](https://user-images.githubusercontent.com/34083808/184832317-a0ee8754-c67f-4efa-b449-5c7338579c31.png)

# Docker advantage

- Only Docker needs to be installed (no other runtimes or tools)
- Uploading our code is very easy  
- It's the exact same app and environment as on our machine.

# "Do-it-yourself" Approach - Disadvantages.

![image](https://user-images.githubusercontent.com/34083808/185070710-412013ea-cc54-4475-8e71-5ccfd4096dca.png)

# A managed / automated approach.

![image](https://user-images.githubusercontent.com/34083808/185074121-7f2b772b-2511-4edd-8e21-dc296a72a782.png)

## Create Amazon ECS (Elastic Container Service)
Step 1: Define Container and task.

![image](https://user-images.githubusercontent.com/34083808/185108964-eae27d72-27f1-4477-bec4-e83b955dde91.png)

We will go with our custom image. 

![image](https://user-images.githubusercontent.com/34083808/185109083-dfa7e96d-0514-468e-b79c-9fd1688fa1b2.png)

then we need to make some setting for your container. such as image repository. 
if you place your image on docker hub, you just need to provide the repository and image name, otherwise you have to provide full path to the site that store that image.

![image](https://user-images.githubusercontent.com/34083808/185109727-aac0e810-52c3-4289-9d9d-ec1abbd9bacf.png)

step 2 Define the service. at them moment skip load balancer.

![image](https://user-images.githubusercontent.com/34083808/185112171-55b1706a-a521-4bde-8d10-923aa97012b3.png)

step 3 Define the cluster. 

![image](https://user-images.githubusercontent.com/34083808/185112547-a1e538dc-77ff-4d1e-b804-97602210d59b.png)

step 4 review our setting again and click on create our ECS instance.

![image](https://user-images.githubusercontent.com/34083808/185113687-7102418f-7aa4-4122-8606-f83434b57fa6.png)

It will take couple of minutes to create a ECS instance. after that you can test by access to public IP adress on port 80.

![image](https://user-images.githubusercontent.com/34083808/185134712-a19d6e82-25a4-40bb-be64-3778c6101838.png)

![image](https://user-images.githubusercontent.com/34083808/185135032-cf9b591b-eaf1-4a9a-8387-6810b683697c.png)

When we want to update the image, we can add another task to current cluster and it will automatically update the latest image and run the server. You can also choose to update on our current task, this mean that we don't need to create new task. 

please delele the service and cluster after you finish. 

![image](https://user-images.githubusercontent.com/34083808/185141242-72e87bc7-ccb6-4131-8aa3-d9caf9a00859.png)

