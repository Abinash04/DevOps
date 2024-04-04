# Docker

# Container ecosystem layers
<img width="900" alt="image" src="https://github.com/Abinash04/DevOps/assets/15240069/0795fcf6-3b12-4f72-a5d3-db5b37e9a1cd">


1. Create your first swarm

![image](https://github.com/Abinash04/DevOps/assets/15240069/f553f75e-a87b-48a7-8588-a7ead5507815)

You can think of Docker Swarm as a special mode that is activated by the command: docker swarm init. 
The --advertise-addr option specifies the address in which the other nodes will use to join the swarm.

This docker swarm init command generates a join token. The token makes sure that no malicious nodes join the swarm.
You need to use this token to join the other nodes to the swarm.

![image](https://github.com/Abinash04/DevOps/assets/15240069/087a76b3-91d5-4fa0-aa29-f93927e4e742)

![image](https://github.com/Abinash04/DevOps/assets/15240069/1171903c-62f7-4db7-850d-90e4dc71b123)


All docker service commands need to be executed on the manager node (Node1).


