> docker network approach will not work on AWS ECS, sinse they may not run the container on the same machine. WE can not use service name as Ip adress for comunication between containers. To be able to use multi container on AWS ECS we need to ajust our image.

then we need to adjust mongodb service name to environment variable. AWS then can use these variable for setting up the connection.

```javascript
mongoose.connect(
  `mongodb://${process.env.MONGODB_USERNAME}:${process.env.MONGODB_PASSWORD}@${process.env.MONGODB_URL}:27017/course-goals?authSource=admin`,
  {
    useNewUrlParser: true,
    useUnifiedTopology: true,
  },
  (err) => {
    if (err) {
      console.error("FAILED TO CONNECT TO MONGODB");
      console.error(err);
    } else {
      console.log("CONNECTED TO MONGODB!!");
      app.listen(80);
    }
  },
);
```

```
MONGODB_USERNAME=max
MONGODB_PASSWORD=secret
MONGODB_URL=mongodb
```

build backend image then push it to docker hub. but before that please create the repository on docker hub.

```
docker build -t goals-node-app ./backend

docker tag goals-node-app yushinb/goals-node-app

docker push yushinb/goals-node-app
```

# Configuring the NodeJS Backend Container.

## Step 1: Create new cluster.

![image](https://user-images.githubusercontent.com/34083808/185147363-e7a0cde6-1666-4b5b-aa8a-a8254e189e4f.png)

## Step 2: Config Cluster and then click on create cluster.

![image](https://user-images.githubusercontent.com/34083808/185147679-6fbfea27-9238-467b-991a-470f5ef33472.png)

wait for couple of minutes for completing.

![image](https://user-images.githubusercontent.com/34083808/185147892-df314f48-e291-49bd-92c8-264f04f20816.png)

## step 3: create task and services.

create new task definition.

![image](https://user-images.githubusercontent.com/34083808/185148648-fb6bc50f-8f82-4dc3-9afe-a65884f2d449.png)

![image](https://user-images.githubusercontent.com/34083808/185148802-59937d5f-6e93-47cf-8ff5-0f1517c00be7.png)

create task definition name and select task role `ecsTaskExecutionRole`

![image](https://user-images.githubusercontent.com/34083808/185149181-8de3ef3b-0b84-4fab-b787-28e5bb4ab8ae.png)

Then choose the smallest amount of memory and vCPU.

![image](https://user-images.githubusercontent.com/34083808/185149410-f2d53401-ce87-4f8e-903d-6821de557642.png)

add container information.

![image](https://user-images.githubusercontent.com/34083808/185149793-9217036a-1192-40e2-85e2-378664df5240.png)

> in this case, you need to add environment variable for our ECS instance.

> in our image we use nodemon for development, but for production we use node, that the reason we add command node, app.js to command field.

> one more think after add environment variable, please not that MondoDB_URL should change from service name in docker compose to local host, because of all container in ECS are in the same task, so they able to comunicate each other by localhost.

> the volume and bind mounth also don't need for production.

![image](https://user-images.githubusercontent.com/34083808/185153769-4e9e8ee9-15e3-4189-a986-030bba67643a.png)

Add another container for mongodb

![image](https://user-images.githubusercontent.com/34083808/185156776-b269980f-90b5-43cd-9c66-9b0ed829615d.png)

The environment variable also need to add same as env/mongo.env file.

```
MONGO_INITDB_ROOT_USERNAME=max
MONGO_INITDB_ROOT_PASSWORD=secret
```

then back to cluster and create new service base on task definition.

![image](https://user-images.githubusercontent.com/34083808/185157950-17335a0f-0ac0-495a-a3f8-11e585a60f14.png)

![image](https://user-images.githubusercontent.com/34083808/185158178-a05d5c46-412e-4916-9f5e-446abbdaf9cd.png)
