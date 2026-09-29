Cloud deployment-->
Key Learnings

1.IAM services(IAM  user link)

2.EC2 (elastic compute cloud)
  steps-
	.go to ec2
	.click on instances then launch instances
	.name the webserver(level5-server) ,select aws like ubuntu.
	.keep all the things same and launch it.
	.now go to instances ,then go to network and security(elastic ips),click allocate elastic ip address.
	.now select the instances and elastic ips click on action then associate ips select the instances name then click on associate
	.now are machine is ready now we have to only deploy the project
	.in instances section select ssh clent  copy the last path paste in the cmd where u saved the level5-kp key pair file

    
3.How to deploy your project
  steps->
	.first clone the git repo write command (git clone git_repo_url)
	.go to your root folder by cd in which your files are saved.
	.install docker in cmd to run the docker file from(docker install ubuntu)
	.run command-> sudo docker build -t level5 .  for building container
	.run the container-> sudo docker run -it -p 5000:5000 level5
	.copy the elastic-ip address of the instances.
	.go to security and network select security and group then add rules enter the port no. i.e 5000 and save.
	.open chrome type url-> http://elastic-ip:port_no.

    
3.If u want to update any thing in your machine code .
  steps->
	.just write what u want and push it on GitHub, but how will u update it on your machine
	.in cmd write -> git pull ,but it will not show in browser so,
	.first stop the running container-> sudo docker stop container_id.
	.then build the image again and run it write->sudo docker build -t level5 .
  Method-2-> for updating changes in the code file so that it will run properly on virtual EC2 machine at any device.
	for this we use CI/CD pipeline.
	.make a folder inside level5 ie .github, inside it make another folder-> workflows,inside this wrkflows folder make a file deploy.yaml
	.now go to github ->settings->secrets and variable->actions(inside this make new repo write ssh_host and ssh_key),also add the elastic ip address in the secret section.,also add the level5kp.pem data in ssh_key.
	.now start and connect the instance copy the ssh path paste it in cmd
	.go to root folder (cd level5)
	.push the changes on github and see the actions (green tick)
	.go to cmd write->sudo docker ps -u will see docker and its image are running properly
	