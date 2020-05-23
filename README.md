# Pré requisito 

* Instalar a última versão do docker
* Sistema operação Linux

# Instalação do gitlab


* Criar variável apontando para um diretóro de volume docker

```bash
export GITLAB_HOME=/home/ricardo/volume/
```

* Instalação do gitlab

```bash 
sudo docker run --detach \
  --hostname gitlab.example.com \
  --publish 443:443 --publish 80:80 --publish 22:22 \
  --name gitlab \
  --restart always \
  --volume $GITLAB_HOME/gitlab/config:/etc/gitlab \
  --volume $GITLAB_HOME/gitlab/logs:/var/log/gitlab \
  --volume $GITLAB_HOME/gitlab/data:/var/opt/gitlab \
  gitlab/gitlab-ce:latest
```

* Editar o arquivo hosts e adicionar a seguinte entrada com seu ip:

```bash
vi /etc/hosts
```
```properties 
192.168.0.8	gitlab.example.com 
127.0.0.1	localhost
```







