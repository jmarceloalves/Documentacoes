# Instalando o docker no Ubuntu

## a) Certifique-se de que não existe nenhuma versão antiga instalada removendo-a
```bash
	sudo apt-get remove docker docker-engine docker.io containerd runc
```

## b) Atualize o repositório
```bash
	sudo apt-get update
```

## c) Instale pacotes necessários
```bash
	sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common -y
```

## d) Certifique-se de que o diretório para chaves de terceiros existe:
```bash
	sudo mkdir -p /etc/apt/keyrings
```

## e) Baixar e converter a chave GPG do Docker
```bash
	curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

## f) Adicionar o repositório com a referência à chave
```bash
	echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## g) Atualize e Instale o Docker
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


## j) Testar a instalação executando o Hello World no terminal
```bash
	docker run hello-world
```

