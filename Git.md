# Comandos Básicos de GIT


## Parte 01 - Trabalhando com o GIT localmente.

### 1. Descobrindo a versão do GIT:
```bash
git version
```

### 2. Instalando o GIT:
```bash
sudo apt update
sudo apt install git
git version
```

### 3. Exibindo as variáveis globais do GIT:
```bash
git config --global --list
```

### 4. Definindo configurações do GIT:
```bash
git config --global user.name "<nome>"
git config --global user.email "<email>"
git config --global init.defaultBranch main # Define o nome padrão para a branch inicial
```

### 5. Inicializar o GIT dentro de um diretório:
```bash
git init # Inicializa um repositório Git
git init -b <nome_branch> # Inicializa um repositório Git e define o nome da branch principal
```

### 6. Exibindo o status do diretório de trabalho e da área de preparação:
```bash
git status
```

### 7. Adicionando arquivos à área de preparação:
```bash
git add .
git add <nome_arquivo>
git add <nome_arquivo_01> <nome_arquivo_02>
```

### 8. Removendo arquivos da área de preparação:
```bash
git rm --cached .
git rm --cached <nome_arquivo>
git rm --cached <nome_arquivo_01> <nome_arquivo_02>
```

### 9. Fazendo um commit:
```bash
git commit -m "<mensagem>"
```

### 10. Visualizando o histórico de commits:
```bash
git log
```

### 11. Listando todas as branchs existentes:
```bash
git branch
```

### 12. Criando uma nova branch:
```bash
git branch <nome_branch>
```

### 13. Navegando entre branchs:
```bash
git switch <nome_branch>
git switch -c <nome_branch> # Cria a branch e muda para ela automaticamente

git checkout <nome_branch>
git checkout -b <nome_branch> # Cria a branch e muda para ela automaticamente
```

### 14. Fazendo merges no GIT:

Existem dois tipos de merge:

	1. merge fast-foward
	2. merge three-way
	
**merge fast-foward:** Um merge fast-forward ocorre quando a branch que você quer integrar (ex: feature) contém todos os commits da branch de destino (ex: main), sem que a branch de destino tenha recebido novos commits nesse intervalo.

**merge three-way:** O three-way merge (merge de três vias) ocorre quando as branches divergiram, ou seja, tanto a branch principal (main) quanto a branch em que você trabalhou (feature) receberam novos commits desde que se separaram.
Diferente do fast-forward, o Git não consegue apenas "arrastar" o ponteiro para a frente. Ele precisa analisar três pontos (por isso o nome) para criar um novo commit de união.

### Fazendo um merge fast-foward:

Antes de executar o merge é preciso alterar o ponteiro do GIT para a branch de destino:
```bash
git checkout <nome_branch_destino>
git merge <nome_branch_origem>
```

### 15. Fazendo checkout de commit
E se após fazer um merge nós quisermos voltar a uma posição específica no passado? Para realizar essa ação é preciso fazer um checkout para um commit em específico. 

O primeiro passo é executar o comando git log para pegar o hash do commit desejado. O comando abaixo simula essa ação:
```bash
git log

commit 237310b3258d11d6d24285926ca4e3a751ecf900
```

Em seguida, podemos fazer o checkout para o commit desejado:
```bash
git checkout 237310b3258d11d6d24285926ca4e3a751ecf900
```

## Parte 02 - Trabalhando com o GIT remotamente.

### 1. Iniciando por um repositório local

O primeiro passo é criar um repositório local. Neste exemplo criei um repositório chamado *rainbow_remote*. Após criar o repositório será preciso copiar a URL para conexão SSH.

O comando abaixo faz a conexão entre o repositório local inicializado e o repositório remoto criado.
```bash
git remote add origin git@localhost:marcelo/rainbow_remote.git
```

O comando abaixo lista todas as conexões remotas existentes para o repositório local:
```bash
git remote
```

O comando abaixo lista todas as conexões remotas existentes para o repositório local com o nome remoto e as URLs:
```bash
git remote -vgit
```


