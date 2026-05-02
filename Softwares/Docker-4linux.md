# Docker
O Docker é uma plataforma Open Source escrita em Go. Uma linguagem de programação de alto desempenho desenvolvida dentro do Google, que facilita a criação e administração de ambientes isolados. Descreve-se como uma plataforma aberta para desenvolvedores e administradores de sistemas para construir, compartilhar e executar aplicações distribuídas. Ele permite que as aplicações sejam executadas em containers.

Existem duas opções para trabalhar com o Docker:
- Community (Gratuita)
- Enterprise (Paga)

## Instalação do Docker no Linux

### Ubuntu
```bash
sudo apt update # Atualiza o indice de pacotes do apt
sudo apt install ca-certificates curl gnupg lsb-release -y # Instala dependências
sudo mkdir -p /etc/apt/keyrings # Cria diretório para armazenar chaves do docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg # Realiza download da chave oficial do Docker
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null #Configurao repositório Docker
sudo apt update # Atualiza o indice de pacotes do apt
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y # Instala o Docker
sudo docker system info # Verifica se o docker foi instalado corretamente
```
Por questões de segurança, o usuário com autorização para configurar o Docker deve fazer parte do grupo docker. Caso você não queira utilizar o comando sudo, toda vez que estiver trabalhando com o Docker, então execute o comando abaixo para adicionar o usuário logado (**$USER**) no grupo docker:
```bash
sudo usermod -aG docker $USER
```
**Obs.:** O comando acima só terá efeito na próxima vez que o usuário logar no servidor.

## Comando essenciais do Docker

O comando ***docker*** irá retornar uma lista de possíveis parâmetros que podem ser utilizados em conjunto:

**Exemplo:**

Management Commands:
- builder:     Manage builds
- buildx*:     Docker Buildx
- compose*:    Docker Compose
- container:   Manage containers
- context:     Manage contexts
- image:       Manage images
- manifest:    Manage Docker image manifests and manifest lists
- network:     Manage networks
- plugin:      Manage plugins
- system:      Manage Docker
- volume:      Manage volumes

O comando ***docker container*** irá retornar uma lista de possíveis parâmetros que podem ser utilizados em conjunto com essa opção. O mesmo raciocínio vale para os demais itens da lista de retorno do comando ***docker***.

### Pesquisando por imagens:
O comando abaixo realiza, via linha de comando, a pesquisa de uma imagem no repositório oficial do Docker (docker hub: https://hub.docker.com/):
```bash
docker search nome_imagem # Ex.: docker search nginx
```
### Baixando uma imagem:
```bash
docker image pull nome_imagem # Ex.: docker image debian
```

### Executando uma container a partir de uma imagem
```bash
docker container run --name nome_servidor --hostname nome_host 
```

### Excluindo imagens e containers
```bash
docker image rm nome_imagem
docker container rm -f nome_container
```
### Listando imagens e containers
```bash
docker image ls -a # O comando **ls** mostra também as demandas que não estão e uso.
docker container ls -a # O comando **ls** mostra também as demandas que não estão e uso.
```
### Iniciando e parando um container
```bash
docker container start nome_container
docker container stop nome_container
```

### Vendo logs de container
```bash
docker container logs nome_container
```
