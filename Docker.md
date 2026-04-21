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

**Observações:**Por quê fizemos isso? Fizemos isso porque caso contrário só poderíamos executar o docker como superusuário, sudo ou root.Não queremos isso. Queremos que nosso usuário comum possa executar o Docker. Por isso adicionamos nosso usuário ao grupo docker. Quando o docker é executado, ele procura por um grupo com seu nome no sistema e dá permissão de uso aos usuários que fazem parte deste grupo. O segundo comando é 'uma gambiarra' que adiciona o nosso usuário a um grupo 'virtual' que demos o nome de docker para evitar que tenhamos que reiniciar o computador.


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