# No Non-Sense Installing Docker on an Azure Linux VM running Jenkins

# Part 2

This article shows how to install [Docker](https://docs.docker.com/engine/install/ubuntu) on an Ubuntu Linux VM for developing, packaging and shipping your containarized apps.

## 1. Install Docker Engine using the convenience script

Docker provides a convenience script at https://get.docker.com/ to install Docker into development environments non-interactively. 
We shall use the scripted installation method for convenience.

// bash command

    curl -fsSL https://get.docker.com -o get-docker.sh
    sudo sh get-docker.sh

To run Docker with non-root privileges you will have to add your user to the docker group

// bash command

    sudo usermod -aG docker $USER
    sudo usermod -aG docker jenkins
    sudo usermod -aG docker azureuser
    sudo usermod -aG docker awsuser

In case the Docker group does not exist create the Docker Group

// bash command

    sudo groupadd docker

Restart so that your group membership is re-evaluated

// bash command

    sudo reboot
    sudo service docker status

## Next steps

> [Jenkins on Azure](./index.yml)
