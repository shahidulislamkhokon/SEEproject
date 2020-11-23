#------------------------------------------ Web base Event Management Application-----------------------------------------------#

1. Require: 
    1. At least 32 GB RAM 64 GB is better
    2. Processor at least core i5
	3. Linux Operating system

2. Software: 
    1. Vagrant (v 2.2.6)
    2. Ansible (v 2.7.5)
	3. maven(v 3.6.2)
	4. virtual box
	
	
	
3. For creating integration server:

Follow the steps below:
  1. for dircetory: cd ~/<git_root_folder>/khokon/pipeline/integration
  2. run vagrant: sudo vagrant up (act as a integration server)
  3. connecet to gitlab: Open http://192.168.34.9/gitlab in your browser.
  
  You need login and password:
  login:root
  password:$YOUR_PASSWORD
  
Using Gitlab:

->Create SEEProduct project
-> to create the remote and local repositories
  1. use the project placed at:~/<git_root_folder>/khokon/FinalProject
  2. Commit and push to your created remote repository.

Build Automate:
At this stage I configured a Gitlab project where developer start the project automatically

Install Git Lab Runner

     1.~/<git_root_folder>/khokon/pipeline/integration (for ensuring)
     2. ssh to the VM:sudo vagrant ssh
	 
For Building FinalProject Register the Runner:
 Follow the steps:
  1. sudo gitlab-runner register
  2. Git URL: http://192.168.34.9/gitlab/
  3. runner enter:  [integration] docker 
  4. For the gitlab-ci tags for this runner enter:buildp
  5. executor:docker
  6. Docker image:alpine:latest
  7. Restart runner:sudo gitlab-runner restart
  
After run these stages you show this massage: Congratualtion Automatic Build process is successfully done


4. stage environment:
  1. Go to the directory:cd ~/<git_root_folder>/khokon/pipeline/stage
  2. Start the staging environment:sudo vagrant up
                                  :sudo vagrant ssh
  3. for Checking Tomcat installation and configuration. Please access the URL:
        1. http://192.168.34.17:8080
The user and password both are: "admin".

If it is not work please restart the tomcat server:
 1. sudo /opt/tomcat/bin/shutdown.sh
 2. sudo /opt/tomcat/bin/startup.sh


5. Deploying to stage server:
    Please follow the steps:
	 1. sudo gitlab-runner register
	 2. GitLab URL: http://192.168.34.9/gitlab/
	 3. description for the runner:shell
	 4. Restart the runner:sudo gitlab-runner restart
     5. Grant sudo permissions:sudo usermod -a -G sudo gitlab-runner
	                          :sudo visudo
     6. Add the the line bottom of the file: gitlab-runner ALL=(ALL) NOPASSWD: ALL
	 7. restart the stage:exit
                         :sudo vagrant reload
						 
Note: Some how the pipeline is not started automatically and 'Auto DevOps' is selected. Please check the settings, go to "Settings" -> "CI/CD" -> "Auto DevOps".	 
          ->cd ~/<git_root_folder>/khokon/FinalProject
		  ->add a line or somethings at 'modify.txt'
		  ->commit and push file to the repository
Please check the below link if the pipeline complete successfully: http://192.168.34.17:8080/FinalProject/

          

6. Setup production environment:
    Pleae follow the setps:
	 1. cd ~/<git_root_folder>/khokon/pipeline/production
	 2. start production environment:sudo vagrant up
                                    :sudo vagrant ssh
	 3. Check tomcat installation and configuration: http://192.168.34.18:8080
The user and password both are: "admin".

If it is not work please restart the tomcat server:
 1. sudo /opt/tomcat/bin/shutdown.sh
 2. sudo /opt/tomcat/bin/startup.sh
 
7. deploying to stage server: 
please follow the commonds:
     1. sudo gitlab-runner register
	 2. GitLab URL: http://192.168.34.9/gitlab/
	 3. description for the runner:shell
	 4. For the gitlab-ci tags:productiond
	 5. For the executor:shell
	 6. Restart the runner:sudo gitlab-runner restart
	 7. Grant sudo permissions:sudo usermod -a -G sudo gitlab-runner
                        :sudo visudo
	 8. Add the the line bottom of the file: gitlab-runner ALL=(ALL) NOPASSWD: ALL
	 9. restart the stage:exit
                         :sudo vagrant reload
						 
Note: Some how the pipeline is not started automatically and 'Auto DevOps' is selected. Please check the settings, go to "Settings" -> "CI/CD" -> "Auto DevOps".	 
          ->cd ~/<git_root_folder>/khokon/FinalProject
		  ->add a line or somethings at 'modify.txt'
		  ->commit and push file to the repository
Please check the below link if the pipeline complete successfully: http://192.168.34.17:8080/FinalProject/
