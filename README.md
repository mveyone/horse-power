# horse-power
3. Add Docker to Sudo group:
https://docs.docker.com/engine/install/linux-postinstall/

# (required) to be able to start minikube
$ sudo groupadd docker

# Add your user to the docker group, replace [user] with your username
$ sudo usermod -aG docker $USER

# To activate changes to the group
$ newgrp docker