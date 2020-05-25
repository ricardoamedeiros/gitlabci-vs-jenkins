# Pré requisito 

* Instalar a última versão do docker
* Sistema operação Linux

# Instalar docker 

```bash
curl https://get.docker.com | bash
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

* Link de acesso: http://localhost:8080


* Selecione os plugins desejados
* Preencha os dados para criar um usuário


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

* Link de acesso: http://gitlab.example.com

* defina a senha

Obs.: O usuário padrão é o root.

# Instalação do gitlab-ci

* Instalação do gitlab-runner onde roda as pipeline do gitlab-ci

```bash
# Pacotes https://gitlab-runner-downloads.s3.amazonaws.com/latest/index.html
curl -LJO https://gitlab-runner-downloads.s3.amazonaws.com/latest/deb/gitlab-runner_amd64.deb
```

```bash
# For Debian/Ubuntu/Mint
dpkg -i gitlab-runner_amd64.deb
```

# Configuração do gitlab-ci

* Registrar a máquina que irá rodar o pipeline

```bash
sudo gitlab-runner register -n \
  --url ${GITLAB_URL} \
  --registration-token ${REGISTRATION_TOKEN} \
  --executor docker \
  --description "Docker Runner" \
  --docker-image "docker:19.03.8" \
  --docker-volumes /var/run/docker.sock:/var/run/docker.sock
```

Exemplo de configuração:

![Exemplo de configuração](image/config-gitlab-ci.png)

Caminho do gitlab onde é obtida a url e o token que devem ser informados ao registrar o gitlab-runner.

Admin Area > Runner

![Exemplo de configuração](image/token.png)

# Habilitar o registry do gitlab

Para o registry funcionar é necessário definir um subdomínio com https. Exemplo: https://registry.gitlab.example.com.

Caso já tenha um subdomínio com https execute os seguintes passos:

* Edite o arquivo gitlab.rb dentro do volume:

```bash
vim $GITLAB_HOME/gitlab/config/gitlab.rb
```

* Descomente e defina a seguinte linha com o subdomnio do gitlab. Exemplo:

registry_external_url 'https://registry.gitlab.example.com'

* Aplique a configuração:

```bash
gitlab-ctl reconfigure
```
Acesse algum projeto e será possível ver a aba **Packages & Registries**:

![Exemplo de configuração](image/registry.png)

**Importante**: Caso não tenha um subdominio com https, o tutorial do link abaixo ensina como gerar um certificado com Lets Encrypt e configurar o registry. 

Link: https://linuxhint.com/gitlab_container_registry_setup

Uma dica para gerar um dns válido é usar o http://xip.io.


# Referências

* https://github.com/jenkinsci/docker/blob/master/README.md
* https://www.jenkins.io/doc/book/pipeline/syntax/
* https://docs.gitlab.com/omnibus/docker/
* https://docs.gitlab.com/runner/install/








