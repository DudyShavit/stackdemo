# Docker Swarm Drills

## Setup
1. Create 2 AWS instances (Free tier)
2. Open The required ports
	* TCP port 2377 for cluster management communications
	* TCP and UDP port 7946 for communication among nodes
	* UDP port 4789 for overlay network traffic
3. SSH into the the instances 
4. Install Docker
	1. curl -fsSL https://get.docker.com -o get-docker.sh
	2. sudo sh get-docker.sh

## Creating the swarm cluster
1. on the manager host:    
	* docker swarm init --advertise-addr YOUR-PRIVATE-IP
2. Copy and save the docker swarm join command
3. on the worker host 
	* copy and run the docker swarm join command
4. check the new swarm cluster on the manager host: 
	* docker node ls
5. check it again using the swarm visualizer 
	1. run it on your manager
		* docker run -it -d -p 8080:8080 -v /var/run/docker.sock:/var/run/docker.sock dockersamples/visualizer
  2. open the 8080 port on the ec2 instance manager
	3. access it from your local PC browser on port 8080

## Pull and Test the stackdemo app on the manager
1. clone the app from  github
	* git clone https://github.com/DudyShavit/stackdemo.git
2. cd stackdemo
3. install docker-compose
	* sudo apt install docker-compose
4. run the application locally on the manager host 
	* sudo docker-compose -f counting-docker-compose.yml up 
5. access it from your local PC browser on the port 8000
	* you can see that this app counting every access 
6. remove the application 
* sudo docker-compose -f counting-docker-compose.yml down

## Run the stackdemo app on the swarm cluster

1. Deploy a new stack:
	 docker stack deploy mystack -c counting-docker-compose.yml
2. use the docker swarm visualizer to show swarm status 
3. access it again from your local PC browser on the port 8000 

## Scale the services to 3 container
1. list your services:  
	* docker service ls
2. scale the redis service to 3: 
	* docker service scale <your_service>=3
3. list your services and make sure they are running on both cluster nodes
* docker service ls
4. use the docker swarm visualizer to show swarm status
5. That`s all! Stop the EC2 instance.

## expected result
![docker_swarm_result](https://user-images.githubusercontent.com/25149394/123789659-14531900-d8e6-11eb-8786-308b77081b7a.png)
