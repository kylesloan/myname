# myname


### Overview
This project shows off 
* ansible automation of a jenkins server
* importing settings for a jenkins job that will use ansible to build a docker image artifact that pushes to AWS S3
* the docker image will take a text file (/usr/local/name.txt with content of `MyFirstName:MyLastName`) and use a shell script to grab the first name, and a python script to grab the last, and echo the results.

### Whats Inside
This project has directories of items that would normally be seperate repositories, but are put into one for ease of learning and following.
* `ansible-jenkins` - ansible settings to configure the AWS EC2 instance so it can run docker, and create the jenkins container and the docker volumes so data persists on docker restarts.  Also starts the docker registry container to make pushes of docker images to S3 easy.
* `ansible-myname` - ansible playbook that jenkin's job will run in order to generate the docker image for our scripts
* `project-myname` - scripts that will be put into the docker image that output the name of the text file.

### Getting Started

##### Clone down the project

* `cd $WhereYouLikeToClone`
* `git clone https://github.com/kylesloan/myname.git`
* `cd myname`

##### Start EC2 instance
This project has been tested on AWS EC2 with the AMI of `Amazon Linux AMI 2016.09.1 (PV) - ami-8a72cdea` (Free Tier). You will need to order a `t1.micro` instance.  Setting up security groups (firewall punches) and ssh key access is beyond the scope of this project, but are also required.

##### Update group_vars variable file
* `cp ansible-jenkins/group_vars/all.yml.example ansible-jenkins/group_vars/all.yml`
* edit `ansible-jenkins/group_vars/all.yml` and follow the directions at the top of the file.


##### Get ansible running
We need ansible to configure the EC2 instance above.  See http://docs.ansible.com/ansible/intro_installation.html for your operating system.

I was able to `brew install ansible` from my mac, running `ansible 2.2.2.0`.  https://brew.sh

Configure the ec2 instance:

* `cd ansible-jenkins`
* `ansible-playbook playbook.yml -i inventory.yml`

##### Manually fire off the jenkins job

* Go to `http://$AWS_IP:8080`
* From the main dashboard (in web browser) click on "myname"
* In the left navigation bar, click "Build Now"
* The "Building History" table below the left bar should create a new entry, click on the #NUMBER
* On the Build Details screen, click on "Console Output".  If the job failed, this output should give you an idea as to what happened.

##### Check S3 for the image

Because docker registry is setup with the S3 information, the docker artifact should now exist in the S3 bucket.  Find this by logging into the AWS console (browser) and select S3 and navigate to the given bucket.


##### Run the myname container

* `ssh -i ~/.ssh/my.pem ec2-user@$IP` -- replace with proper IP and path to PEM
* `sudo docker images`
* `sudo docker run localhost:5000/myname`

##### Run the myname container with a volume replacement of the name.txt file

* `echo "Leeroy:Jenkins" > mynewname.txt`
* `sudo docker run -v $(pwd)/mynewname.txt:/usr/local/name.txt localhost:5000/myname`
