#local do volume definido na variável GITLAB_HOME
export GITLAB_HOME=/home/ricardo/volume/

#Instalação do gitlab
sudo docker run --detach \
  --hostname gitlab.example.com \
  --publish 443:443 --publish 80:80 --publish 22:22 \
  --name gitlab \
  --restart always \
  --volume $GITLAB_HOME/gitlab/config:/etc/gitlab \
  --volume $GITLAB_HOME/gitlab/logs:/var/log/gitlab \
  --volume $GITLAB_HOME/gitlab/data:/var/opt/gitlab \
  gitlab/gitlab-ce:latest

#Editar o arquivo hosts e adicionar a seguinte entrada com seu ip:

vi /etc/hosts

192.168.0.8	gitlab.example.com 



