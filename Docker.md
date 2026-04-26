## Instalando o docker no Ubuntu

### a) Certifique-se de que não existe nenhuma versão antiga instalada removendo-a
```bash
	sudo apt-get remove docker docker-engine docker.io containerd runc
```

### b) Atualize o repositório
```bash
	sudo apt-get update
```

### c) Instale pacotes necessários
```bash
	sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common -y
```

### d) Certifique-se de que o diretório para chaves de terceiros existe:
```bash
	sudo mkdir -p /etc/apt/keyrings
```

### e) Baixar e converter a chave GPG do Docker
```bash
	curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

### f) Adicionar o repositório com a referência à chave
```bash
	echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### g) Atualize e Instale o Docker
```bash
	sudo apt update
	sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## h) Verifique se o Docker foi corretamente instalado executando no terminal
```bash
	docker version
```

## i) Vamos adicionar o nosso usuário ao grupo do docker
```bash
	sudo usermod -aG docker $USER

	newgrp docker
```

**Observações:** Por quê fizemos isso? Fizemos isso porque caso contrário só poderíamos executar o docker como superusuário, sudo ou root.Não queremos isso. Queremos que nosso usuário comum possa executar o Docker. Por isso adicionamos nosso usuário ao grupo docker. Quando o docker é executado, ele procura por um grupo com seu nome no sistema e dá permissão de uso aos usuários que fazem parte deste grupo. O segundo comando é 'uma gambiarra' que adiciona o nosso usuário a um grupo 'virtual' que demos o nome de docker para evitar que tenhamos que reiniciar o computador.


### j) Testar a instalação executando o Hello World no terminal
```bash
	docker run hello-world
```

## Entendendo imagens e containers

Imagens Docker são templates somente-leitura (modelos) contendo código, bibliotecas e dependências, funcionando como "receitas" para criar pacotes de software. Containers são instâncias executáveis dessas imagens. Enquanto a imagem é o "manifesto", o container é o ambiente isolado onde a aplicação roda, garantindo consistência entre diferentes computadores. 


**O que são Imagens Docker**

**Definição:** Um pacote leve, independente e executável que inclui tudo necessário para rodar uma aplicação.

**Estrutura:** Compostas por várias camadas (layers) empilhadas, sendo somente-leitura.

**Exemplo:** Uma imagem Ubuntu com Apache e seu código PHP pré-instalado.

**Analogia:** É o "molde" ou "planta" de um bolo. 


**O que são Containers Docker**

**Definição:** Uma instância de tempo de execução (runtime) de uma imagem; é a imagem em funcionamento.

**Característica:** Leves, portáteis e isolados do sistema operacional hospedeiro.

**Exemplo:** Vários containers rodando o mesmo site, cada um iniciado a partir da mesma imagem.

**Analogia:** É o "bolo" assado e pronto para consumo. 


**Principais Diferenças e Sínonimos**


| **Característica** | **Imagem Docker** | **Container Docker**
| :--- | :--- | :--- 
| Estado | "Estática" (Somente-leitura) | "Dinâmica" ( Rodando)
| Função | Armazenar/empacotar | Executar/Rodar
| Sínonimos | Template, Modelo, Manifesto | Instância, Ambiente, Máquina virtual leve

## Principais comandos no docker

```bash
docker --version
docker --help
```

### Listar todas as imagens existentes no servidor
```bash
docker image ls
```

### Deletar uma imagem do disco
```bash
docker rmi <id_imagem>
```

### Listar todos os containers em execução
```bash
docker container ls # Sintaxe moderna
docker ps # Sintaxe legado
```

### Listar todos os containers (em execução + parados)
```bash
docker ps -a
```

### Deletar um container docker
```bash
docker rm <id_do_container>
```

### Iniciar um container
```bash
docker container start <id_do_container>
```

### Configurando uma imagem do docker para iniciar junto com o sistema operacional
```bash
docker update --restart always <id_container>
```

### Fazendo um backup de uma imagem
```bash
docker image save -o <nome_arquivo.tar> <nome_imagem>
```

### Restaurando uma imagem a partir de um backup
```bash
docker load -i <nome_arquivo.tar>
```

## Trabalhando com Volumes

### Acessando um container de forma interativa (bash)
```bash
docker exec -it <nome_container> sh # Para sair do container aperte CTRL+D
```

### Persistindo volumes
```bash
docker run --name "nome_servidor" -d -p 8080:80 -e NGINX_ENTRYPOINT_QUIET_LOGS=1 -v "/home/marcelo/Downloads/unit6:/usr/share/nginx/html" nginx:1.19.4-alpine
```

A primeira parte do comando ***docker run --name "nome_servidor" -d -p 8080:80*** inicializa uma imagem do docker e mapeia que a porta 8080 da máquina host será mapeado para a porta 80 do container.

A segunda parte ***-v "/home/marcelo/Downloads/unit6:/usr/share/nginx/html"*** mapeia uma pasta local (/home/marcelo/Downloads/unit6) para a pasta /usr/share/nginx/html do container.

Para identificar quais os mapeamentos de diretórios existentes entre a máquina host e o container utilizamos o comando:
```bash
docker inspect <id_do_container>
```

As  informações podem ser validadas na seção "Mounts" da saída do comando

### Trabalhando com Dockerfiles

Um **Dockerfile** nada mais é do que um documento de texto, no formato **YAML**, com instruções que são utilizadas pelo **Docker** para gerar uma imagem baseada nas instruções do documento.

Essa imagem gerada a partir do nosso Dockerfile pode ser publicada no Docker Hub e compartilhada com outras pessoas.

### Criando um Dockerfile simples

**1.** O primeiro passo é criar um arquivo chamado **Dockerfile**. Este arquivo não precisa ter extensão, caso estejamos trabalhando com um único Dockerfile. No caso de mais de um arquivo devemos seguir o padrão funcao.dockerfile (Ex.: postgresql.dockerfile, nginx.dockerfile,...).

Abaixo temos um exemplo de um arquivo Dockerfile simples escrito em YAML:
```bash
FROM nginx:1.19.4-alpine
LABEL maintainer="Geek University <contato@geekuniversity.com.br>"
COPY . /usr/share/nginx/html
EXPOSE 80
```

A primeira linha faz referência à imagem do docker hub.

A segunda linha cria um label com o nome e contato do responsável por essa imagem.

A terceira linha irá copiar todo o conteúdo do diretório onde o dockerfile está localizado para o diretório /usr/share/nginx/html na imagem.

A quarta linha informa que a porta 80 da imagem deverá ser liberada.


**2.** Após criarmos o arquivo, devemos "buildar" o mesmo. "Buildar" significa criar a imagem no nosso computador:
```bash
docker build -f Dockerfile -t jmarceloalves/servidor_web:v1 .
```

O comando **-f Dockerfile** aponta para o nome do arquivo (Dockerfile). Se o nome do arquivo fosse postgresql.dockerfile o comando seria **-f postgresql.dockerfile**.

O comando **-t jmarceloalves/servidor_web:v1** cria uma tag, informando o meu nome de usuário do docker hub, o nome como essa imagem será chamada (servidor_web) e a versão desta imagem (v1).

O comando **.** informa ao comando **docker build** aonde o arquivo **dockerfile** está salvo. No nosso caso, **.** significa que o arquivo está no mesmo diretório onde estamos rodando nosso bash.

**3.** Gerando a imagem
```bash
docker run --name "servidor_web" -d -p 8080:80 jmarceloalves/servidor_web:v1
```

No comando acima estamos criando um container chamado servidor_web, direcionando a porta 80 do container para a porta 8080 da nossa máquina e estamos informando o nome da imagem existente no nosso servidor. 



















**Exemplo:** Instalando imagem do PostgreSQL

```bash
sudo apt update
docker pull postgres:18.3 # Imagem oficial do PostgreSQL
docker run --name postgres18Docker \
  -e POSTGRES_PASSWORD=123 \
  -p 5432:5432 \
  -d postgres:18.3
 ```
  
Ao executar os comandos acima o PostgreSQL estará disponível para rodar no seu servidor.