# Pré requisito 

* Instalar a última versão do docker
* Sistema operação Linux

# Instalar docker 

```bash
curl -fsSL https://get.docker.com | sudo sh get-docker.sh
```

# Instalação do gitlab


* Criar variável apontando para um diretóro de volume docker

```bash
export GITLAB_HOME=~/volume/
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

* Editar o arquivo hosts e adicionar a entrada gitlab.example.com com seu ip. Exemplo:

```bash
vi /etc/hosts
```
```properties 
192.168.0.8	gitlab.example.com 
127.0.0.1	localhost
```

# Instalação do Jenkins


* Criar variável apontando para um diretóro de volume docker

```bash
export JENKINS_HOME=~/volume
mkdir ~/volume/jenkins
```

```bash
docker run -d -v $JENKINS_HOME/jenkins:/var/jenkins_home -p 8080:8080 -p 50000:50000 --name jenkins jenkins/jenkins:lts
```

* Comando para verificar o log do jenkins e recuperar o token de acesso.

```bash
 docker logs jenkins
```

Exemplo de token:

91f4600e9d5d437da7e576ffc91ffd6a


* Selecione os plugins desejados






# Referências

* https://github.com/jenkinsci/docker/blob/master/README.md

* https://docs.gitlab.com/omnibus/docker/







