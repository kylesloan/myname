Instructions:


Using EC2 t2.micro instances,

Create/configure Jenkins server (only simple configurations needed here)

Create a text file with a single line that has your first and last name separated by ":"

Code up 2 applications/scripts and push to a Github repo:
- one in bash that prints your first name extracted from the text file you created earlier
- one in python/java/golang (pick one) that prints your last name extracted from the text file you created earlier

Create a job that will
- take code changes merged from the Github repo (this needs to support future code merges)
- create an Ansible Playbook
--- build it into a Docker container such that it outputs both first and last names from the scripts
--- store the container artifact into an S3 bucket


terminate ec2 instances and s3 buckets so no charges are incurred


Will need:

AWS Free Tier account (https://aws.amazon.com/s/dm/optimization/server-side-test/free-tier/free_np/)
Github account
