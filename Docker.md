# Docker

Course labs
By completing these labs, you'll get hands-on experience creating your first Docker container. Each lab is followed by a 
lab solution video in case you have questions or can't complete the lab.

Lab 1: Run your first container and learn how to inspect it. Explore the Docker Hub and discover common images ready to be run.

Lab 2: Create a custom Docker Image built from a Dockerfile and push it to a central registry where it can be pulled to be
deployed on other environments. Learn about the union file system and copy-on-write, and how they help you deliver applications faster.

Lab 3: Deploy containers using Docker swarm and learn how Docker Swarm helps solve problems such as reconciliation, scaling, high availability 
and service discovery. In this lab, you'll use Play-with-Docker for a multi-node cluster rather than use your locally installed Docker.

https://labs.play-with-docker.com/
Play with docker is a free web-based free Alpine Linux Virtual Machine in the cloud , where you can build and run Docker containers and even create clusters with Docker features like Swarm Mode.

# Containers

<img width="691" alt="image" src="https://github.com/Abinash04/DevOps/assets/15240069/b0e4338b-b80e-4f16-989e-775b068a9137">

# VM vs Containers

<img width="756" alt="image" src="https://github.com/Abinash04/DevOps/assets/15240069/d0d86efc-3679-401d-9899-77cf259dd34d">

# docker

<img width="788" alt="image" src="https://github.com/Abinash04/DevOps/assets/15240069/a62a6b32-d06a-496b-9bb5-71c01c2d4d0d">

# Why containers are applealing to users

<img width="814" alt="image" src="https://github.com/Abinash04/DevOps/assets/15240069/f1c8770a-3b94-4dac-99f1-7716f8c71a60">

# Run a container
```
$ docker container run -t ubuntu top
$ docker container ls 
$ docker container exec -it b3ad2a23fab3 bash
$ ps -ef
```

Other Linux namespaces include:

MNT: Mount and unmount directories without affecting other namespaces.
NET: Containers have their own network stack.
IPC: Isolated interprocess communication mechanisms such as message queues.
User: Isolated view of users on the system.
UTC: Set hostname and domain name per container.
These

Tip: Namespaces are a feature of the Linux kernel. However, Docker allows you to run containers on Windows and Mac. 
The secret is that embedded in the Docker product is a Linux subsystem. Docker open-sourced this Linux subsystem to a 
new project: LinuxKit. Being able to run containers on many different platforms is one advantage of using the Docker tooling with containers.

# Docker Hub

The Docker Hub is the public central registry for Docker images. Anyone can share images here publicly. The Docker Store contains 
community and official images that can also be found on the Docker Hub.

When searching for images, you will find filters for Store and Community images. Store images include content that has been verified 
and scanned for security vulnerabilities by Docker. Go one step further and search for Certified images that are deemed enterprise-ready 
and are tested with Docker Enterprise Edition.

It is important to avoid using unverified content from the Docker Store when you develop your own images that are intended to be deployed 
into the production environment. These unverified images might contain security vulnerabilities or possibly even malicious software.

- Run NGINX server by using official NGINX image from docker store:
  
```
$ docker container run --detach --publish 8080:80 --name nginx nginx
$ docker container run --detach --publish 8081:27017 --name mongo mongo:3.4

```

Remember: You didn't have to install anything on your host (other than Docker) to run these processes!
Each container includes the dependencies that it needs within the container, so you don't need to install anything on your host directly.

Running multiple containers on the same host gives us the ability to use the resources (CPU, memory, and so on) 
available on single host. This can result in huge cost savings for an enterprise.

Although running images directly from the Docker Store can be useful at times, it is more useful to create custom images 
and refer to official images as the starting point for these images. 

# remove containers
```
$ docker container ls
$ docker container stop [container id]
$ docker container stop d67 ead af5
d67
ead
af5
Tip: You need to enter only enough digits of the ID to be unique. Three digits is typically adequate.
$ docker system prune
```
# Lab1 Summary:
Containers are composed of Linux namespaces and control groups that provide isolation from other containers and the host.

Because of the isolation properties of containers, you can schedule many containers on a single host without 
worrying about conflicting dependencies. This makes it easier to run multiple containers on a single host: 
using all resources allocated to that host and ultimately saving server costs.

That you should avoid using unverified content from the Docker Store when developing your own images because these 
images might contain security vulnerabilities or possibly even malicious software.
Containers include everything they need to run the processes within them, so you don't need to install additional dependencies on the host.

# Build docker image
```
echo 'from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    return "hello world!"

if __name__ == "__main__":
    app.run(host="0.0.0.0")' > app.py

Dockerfile contents below:

FROM python:3.6.1-alpine
RUN pip install --upgrade pip
RUN pip install flask
CMD ["python","app.py"]
COPY app.py /app.py

$ docker image build -t python-hello-world .

$ docker image ls
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
python-hello-world   latest              f1b2781b3111        26 seconds ago      99.3MB
python               3.6.1-alpine        c86415c03c37        8 days ago          88.7MB

Run the Docker image:

$ docker run -p 5001:5000 -d python-hello-world
0b2ba61df37fb4038d9ae5d145740c63c2c211ae2729fc27dc01b82b5aaafa26

$ docker container logs [container id] 
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
172.17.0.1 - - [28/Jun/2017 19:35:33] "GET / HTTP/1.1" 200 -

```

# Push to central repository
![image](https://github.com/Abinash04/DevOps/assets/15240069/5f1334d0-28b4-4f84-9185-881f89f07bc0)

![image](https://github.com/Abinash04/DevOps/assets/15240069/08440288-b373-4488-9d4b-2cd1a98b96ea)


Tag the image with your username.

The Docker Hub naming convention is to tag your image with [dockerhub username]/[image name]. To do this, tag your previously 
created image python-hello-world to fit that format.

$ docker tag python-hello-world [dockerhub username]/python-hello-world
After you properly tag the image, use the docker push command to push your image to the Docker Hub registry:

$ docker push jzaccone/python-hello-world
The push refers to a repository [docker.io/jzaccone/python-hello-world]
2bce026769ac: Pushed 
64d445ecbe93: Pushed 
18b27eac38a1: Mounted from library/python 
3f6f25cd8b1e: Mounted from library/python 
b7af9d602a0f: Mounted from library/python 
ed06208397d5: Mounted from library/python 
5accac14015f: Mounted from library/python 
latest: digest: sha256:508238f264616bf7bf962019d1a3826f8487ed6a48b80bf41fd3996c7175fd0f size: 1786
Check your image on Docker Hub in your browser.

Navigate to Docker Hub and go to your profile to see your uploaded image.

Now that your image is on Docker Hub, other developers and operators can use the docker pull command to deploy your image to other environments.

Remember: Docker images contain all the dependencies that they need to run an application within the image. 
This is useful because you no longer need to worry about environment drift (version differences) when you rely on 
dependencies that are installed on every environment you deploy to. You also don't need to follow more steps to provision these environments. Just one step: install docker, and that's it.

# deploy a change
<img width="791" alt="image" src="https://github.com/Abinash04/DevOps/assets/15240069/587b30c0-f82e-4bd7-b505-4a370a31a1c7">
<img width="791" alt="image" src="https://github.com/Abinash04/DevOps/assets/15240069/1b7ebb15-fba5-4a4d-846d-751bdbb36b3c">

<img width="791" alt="image" src="https://github.com/Abinash04/DevOps/assets/15240069/f1026914-9e15-47ec-8639-3cebb68761f6">
<img width="791" alt="image" src="https://github.com/Abinash04/DevOps/assets/15240069/3af8f9a0-54ab-49dd-95ab-a4017824ae15">

# remove containers
<img width="791" alt="image" src="https://github.com/Abinash04/DevOps/assets/15240069/df3f264a-bf59-442e-a5d8-1fee4129be97">


# Lab 2 summary
In this lab, you started adding value by creating your own custom Docker containers. Remember these key points:

Use the Dockerfile to create reproducible builds for your application and to integrate your application with Docker into the CI/CD pipeline.

Docker images can be made available to all of your environments through a central registry. The Docker Hub is one example of a registry,
but you can deploy your own registry on servers you control.
A Docker image contains all the dependencies that it needs to run an application within the image. This is useful because you no longer need
to deal with environment drift (version differences) when you rely on dependencies that are installed on every environment you deploy to.

Docker uses of the union file system and "copy-on-write" to reuse layers of images. This lowers the footprint of storing images and significantly 
increases the performance of starting containers.
Image layers are cached by the Docker build and push system. There's no need to rebuild or repush image layers that are already present on a system.

Each line in a Dockerfile creates a new layer, and because of the layer cache, the lines that change more frequently, for example, adding source code to an image, should be listed near the bottom of the file.

# Container ecosystem layers
<img width="900" alt="image" src="https://github.com/Abinash04/DevOps/assets/15240069/0795fcf6-3b12-4f72-a5d3-db5b37e9a1cd">


1. Create your first swarm

```
[node1] (local) root@192.168.0.18 ~
$ docker swarm init --advertise-addr eth0
```

![image](https://github.com/Abinash04/DevOps/assets/15240069/f553f75e-a87b-48a7-8588-a7ead5507815)

You can think of Docker Swarm as a special mode that is activated by the command: docker swarm init. 
The --advertise-addr option specifies the address in which the other nodes will use to join the swarm.

This docker swarm init command generates a join token. The token makes sure that no malicious nodes join the swarm.
You need to use this token to join the other nodes to the swarm.

```
docker swarm join --token SWMTKN-1-3349njqt8f2g16lsygtmthipbsjcmb1gmiz5fe2tdmm6j6fu0g-0ogznrgbci1r30f9lv94skzux 192.168.0.18:2377
```

![image](https://github.com/Abinash04/DevOps/assets/15240069/087a76b3-91d5-4fa0-aa29-f93927e4e742)

![image](https://github.com/Abinash04/DevOps/assets/15240069/1171903c-62f7-4db7-850d-90e4dc71b123)


All docker service commands need to be executed on the manager node (Node1).


2. Deploy your first service
   Now that we have 3 node swarm cluster initialized, we'll deploy some containers. To run containers on a docker swarm, we need to create a service.
   A service is an abstraction that represents multiple containers of the same image deployed across distributed cluster.

   Let's deploy NGINX service with one running container which later we will scale up.

   - On Node1 (manager node), deploy a service by using NGINX.
     
```
 $ docker service create --detach=true --name nginx1 --publish 80:80 --mount source=/etc/hostname,target=/usr/share/nginx/html/index.html,type=bind,ro nginx:1.12

[node1] (local) root@192.168.0.18 ~
$ docker service create --detach=true --name nginx1 --publish 80:80 --mount source=/etc/hostname,target=/usr/share/nginx/html/index.html,type=bind,ro nginx:1.12
rryobxi73iyp0at64507eton0
[node1] (local) root@192.168.0.18 ~
$ 
```

This command statement is declarative, and Docker Swarm will try to maintain the state declared in this command unless explicitly 
changed by another docker service command. This behavior is useful when nodes go down, for example, and containers are automatically 
rescheduled on other nodes.

The --mount flag is useful to have NGINX print out the hostname of the node it's running on.
We are using NGINX tag 1.12 in this command. We will see a rolling update with version 1.13 later.

The --publish command uses the swarm's built-in routing mesh. In this case, port 80 is exposed on every node in the swarm. 
The routing mesh will route a request coming in on port 80 to one of the nodes running the container.

- Inspect the service. 

![image](https://github.com/Abinash04/DevOps/assets/15240069/d8f9766f-404f-4b86-9287-65a84283d39a)

- Check the running container of the service.
  ![image](https://github.com/Abinash04/DevOps/assets/15240069/89638088-27ac-4960-8bb4-a9f09b316b48)

- Test the service

  Because of the routing mesh, you can send a request to any node of the swarm on port 80. This request will be
  automatically routed to the one node that is running the NGINX container.

  Try this command on each node:
```
$ curl localhost:80
```
![image](https://github.com/Abinash04/DevOps/assets/15240069/7e620314-095c-4eb3-a4c3-120168c804fc)

Curling will output the hostname where the container is running. For this example, it is running on node2.

3. Scale your service

In production, you might need to handle large amounts of traffic to your application, so you'll learn how to scale.

- Update your service with an updated number of replicas.

Use the docker service command to update the NGINX service that you created previously to include 5 replicas. This is defining a new state for the service.

![image](https://github.com/Abinash04/DevOps/assets/15240069/6e4753b5-9efd-4856-a0a9-064d5a2f1c92)

When this command is run, the following events occur:

The state of the service is updated to 5 replicas, which is stored in the swarm's internal storage.
Docker Swarm recognizes that the number of replicas that is scheduled now does not match the declared state of 5.
Docker Swarm schedules 5 more tasks (containers) in an attempt to meet the declared state for the service.
This swarm is actively checking to see if the desired state is equal to actual state and will attempt to reconcile if needed.

- Check the running instances.

After a few seconds, you should see that the swarm did its job and successfully started 9 more containers. 
Notice that the containers are scheduled across all three nodes of the cluster. The default placement strategy 
that is used to decide where new containers are to be run is the emptiest node, but that can be changed based on your needs.

- Send a lot of requests to http://localhost:80.

The --publish 80:80 parameter is still in effect for this service; that was not changed when you ran the docker service 
update command. However, now when you send requests on port 80, the routing mesh has multiple containers in which to 
route requests to. The routing mesh acts as a load balancer for these containers, alternating where it routes requests to.

Try it out by curling multiple times. Note that it doesn't matter which node you send the requests. There is no connection
between the node that receives the request and the node that that request is routed to.

![image](https://github.com/Abinash04/DevOps/assets/15240069/c00069f3-ebf2-4866-be63-453aac8d39dd)

You should see which node is serving each request because of the useful --mount command you used earlier.

Limits of the routing mesh: The routing mesh can publish only one service on port 80. If you want multiple 
services exposed on port 80, you can use an external application load balancer outside of the swarm to accomplish this.

- Check the aggregated logs for the service.

Another easy way to see which nodes those requests were routed to is to check the aggregated logs. 
You can get aggregated logs for the service by using the command docker service logs [service name]. 
This aggregates the output from every running container, that is, the output from docker container logs [container name].

![image](https://github.com/Abinash04/DevOps/assets/15240069/8c3097bf-2828-4732-adc5-5662e1fa2b31)

Based on these logs, you can see that each request was served by a different container.

In addition to seeing whether the request was sent to node1, node2, or node3, you can also see which container 
on each node that it was sent to. For example, nginx1.5 means that request was sent to a container with that same name 
as indicated in the output of the command docker service ps nginx1.


4. Apply rolling updates

   Now that you have your service deployed, you'll see a release of your application. You are going to update the version of NGINX to version 1.13.

Run the docker service update command:

$ docker service update --image nginx:1.13 --detach=true nginx1
This triggers a rolling update of the swarm. Quickly enter the command docker service ps nginx1 over and over to see the updates in real time.

You can fine-tune the rolling update by using these options:

--update-parallelism: specifies the number of containers to update immediately (defaults to 1).
--update-delay: specifies the delay between finishing updating a set of containers before moving on to the next set.
After a few seconds, run the command docker service ps nginx1 to see all the images that have been updated to nginx:1.13.

$ docker service ps nginx1

![image](https://github.com/Abinash04/DevOps/assets/15240069/4be14e25-5565-4fa3-8c65-48851287a16a)


5. Reconcile problems with containers

In the previous section, you updated the state of your service by using the command docker service update. You saw Docker Swarm in action as it recognized the mismatch between desired state and actual state, and attempted to solve the issue.

The inspect-and-then-adapt model of Docker Swarm enables it to perform reconciliation when something goes wrong. For example, when a node in the swarm goes down, it might take down running containers with it. The swarm will recognize this loss of containers and will attempt to reschedule containers on available nodes to achieve the desired state for that service.

You are going to remove a node and see tasks of your nginx1 service be rescheduled on other nodes automatically.

To get a clean output, create a new service by copying the following line. Change the name and the publish port to avoid conflicts with your existing service. Also, add the --replicas option to scale the service with five instances:
$ docker service create --detach=true --name nginx2 --replicas=5 --publish 81:80  --mount source=/etc/hostname,target=/usr/share/nginx/html/index.html,type=bind,ro nginx:1.12
aiqdh5n9fyacgvb2g82s412js

By mistakenly ran the above cmd with wrong image so had to remove the service like below.

![image](https://github.com/Abinash04/DevOps/assets/15240069/b3c1a904-b227-4d56-bb3b-eabfb7fd27db)

On node1, use the watch utility to watch the update from the output of the docker service ps command.
Tip: watch is a Linux utility and might not be available on other operating systems.

$ watch -n 1 docker service ps nginx2

![image](https://github.com/Abinash04/DevOps/assets/15240069/b1b062c4-c087-4169-b228-3a8d044f4a9e)


Press Ctrl+C to return to command prompt.
Click node3 and enter the command to leave the swarm cluster:
$ docker swarm leave
Tip: This is the typical way to leave the swarm, but you can also kill the node and the behavior will be the same.

![image](https://github.com/Abinash04/DevOps/assets/15240069/fb8e0926-3c75-4a0e-b618-35d3e7915d0f)

Click node1 to watch the reconciliation in action. You should see that the swarm attempts to get back to the declared state by rescheduling the containers that were running on node3 to node1 and node2 automatically.

![image](https://github.com/Abinash04/DevOps/assets/15240069/6e9a4308-fde2-4ab0-861f-c56a19d77ed7)


6. Determine how many nodes you need
   
In this lab, your Docker Swarm cluster consists of one master and two worker nodes. This configuration is not highly available. 
The manager node contains the necessary information to manage the cluster, but if this node goes down, the cluster will cease to 
function. For a production application, you should provision a cluster with multiple manager nodes to allow for manager node failures.

You should have at least three manager nodes but typically no more than seven. Manager nodes implement the raft consensus 
algorithm, which requires that more than 50% of the nodes agree on the state that is being stored for the cluster. If you don't 
achieve more than 50% agreement, the swarm will cease to operate correctly. For this reason, note the following guidance for node failure tolerance:

Three manager nodes tolerate one node failure.
Five manager nodes tolerate two node failures.
Seven manager nodes tolerate three node failures.
It is possible to have an even number of manager nodes, but it adds no value in terms of the number of node failures. 
For example, four manager nodes will tolerate only one node failure, which is the same tolerance as a three-manager node cluster. 
However, the more manager nodes you have, the harder it is to achieve a consensus on the state of a cluster.

While you typically want to limit the number of manager nodes to no more than seven, you can scale the number of worker nodes 
much higher than that. Worker nodes can scale up into the thousands of nodes. Worker nodes communicate by using the gossip protocol, 
which is optimized to be perform well under a lot of traffic and a large number of nodes.


# Lab3 Summary:
```
In this lab, you got an introduction to problems that come with running containers in production, such as scheduling services
across distributed nodes, maintaining high availability, implementing reconciliation, scaling, and logging. You used the orchestration
tool that comes built-in to the Docker Engine, Docker Swarm, to address some of these issues.

Remember these key points:

Docker Swarm schedules services by using a declarative language. You declare the state, and the swarm attempts to maintain and
reconcile to make sure the actual state equals the desired state.
Docker Swarm is composed of manager and worker nodes. Only managers can maintain the state of the swarm and accept commands to modify it.

Workers have high scalability and are only used to run containers. By default, managers can also run containers.
The routing mesh built into Docker Swarm means that any port that is published at the service level will be exposed on every node in the swarm.

Requests to a published service port will be automatically routed to a container of the service that is running in the swarm.
You can use other tools to help solve problems with orchestrated, containerized applications in production, including Docker Swarm
```
