# Running a Django webapp inside a Docker container on AWS EC2 instance
## How to containerize a django application and run it on docker

I used a django todo app by made by [Shrey Shah](https://github.com/shreys7),  [web application](https://github.com/shreys7/django-todo) and I tried to containerize it. 

Here are the steps that I followed:
1. Create an EC2 instance using AWS with *Base image* as Linux 
2. Launch the instance
3. Launch the instance using ssh and .pem file
4. After connecting , make a directory as project.
5. goto project and clone the repository here

![image](https://user-images.githubusercontent.com/68144680/206733373-0e08557e-02fa-49c7-9992-f315cd6ba243.png)

6. Now set the `security groups` of our EC2 instance. We have to `set inbound rules to allow traffic on port no 8000`.

For that we will go to **security group** and edit the **inbound rules**
7. add the rule with the name custom-TCP and allow the traffic on ipv4, `0.0.0.0`  port `8001`

![image](https://user-images.githubusercontent.com/68144680/206734558-050f4854-cefb-463f-82a2-6915a85e289f.png)

## Creating a *Dockerfile*
we have to create docker image so that it can be run in a container
1. install Docker on our EC2 instance
  `sudo apt install docker.io`
  `vi Dockerfile`
### understanding the Dockerfile
   ```
   >>FROM python3
    1. This will create the **image** of OS i.e. ubuntu where python3 is already installed.

    ## For dependencies  of python
    //pip install django==3.2
    since , it is a cmd
    >>RUN pip install django==3.2
    ## Now we have to put the code in the box.
    >>COPY . .

    1st (dot). → source (code from src where our docker file exists)

    2nd (dot). → destination (container )
    
    >>`RUN python manage.py migrate`

    >>`CMD [ "python", "manage.py", "runserver", "0.0.0.0:8001"]`

    //CMD is in the form of an array
```

![image](https://user-images.githubusercontent.com/68144680/206735617-3d4967b3-a30d-40b1-93ac-3710548d52c3.png)

2. //now build our dockerfile which will build our image

3. `docker build . -t todoapp`

**t → tag name**

*ps: it is python:3 in dockerfile*
`FROM python:3`

![image](https://user-images.githubusercontent.com/68144680/206735974-ae6af9ac-027f-459f-bd05-5d954d696f6b.png)

![image](https://user-images.githubusercontent.com/68144680/206736104-1f1c73b4-ba42-4b5f-a7ba-4ca9e8406684.png)

`d73552460333` : this is our image id
<br>
now to see if our image is running or not we will use the following command.
<br>
`sudo docker ps`
<br>
//we have to map the port of our system with docker image
<br>
`sudo docker run -p 8001:8001 d73552460333`

![image](https://user-images.githubusercontent.com/68144680/206736407-07c4892f-2c0a-4bc2-b50c-d82dc3c98eda.png)
## *HURRAYYYYYYYYY! 🙌🙌🧡*

### Now our application is running 

![image](https://user-images.githubusercontent.com/68144680/206736689-5ccecdc7-fbdd-4ea0-a07b-13b2585ce100.png)

### tip:
 //to run it in background , you can use the docker run  command by simply adding -d
<br>
i.e.  docker daemon, and docker daemon runs in the background

![image](https://user-images.githubusercontent.com/68144680/206741306-40f6f176-6119-4654-b10f-b99cb407cbe3.png)

