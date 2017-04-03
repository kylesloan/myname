# https://hub.docker.com/_/jenkins/
FROM jenkins:alpine
USER root

# We need all these guys to be able to run ansible and do builds back to the docker engine box.
# Do we really need all the packages? Maybe not and would be a good thing to check later.
# Was getting error when jenkins was building myname container.
RUN apk update && apk add ansible docker docker-py py-pip python python-dev build-base
RUN pip install docker-py

# Allows for more auto install of plugins if we had a more complex jenkins setup, see hub link.
#COPY plugins.txt /usr/share/jenkins/plugins.txt
#RUN /usr/local/bin/plugins.sh /usr/share/jenkins/plugins.txt
