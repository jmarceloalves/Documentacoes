# Instalando e configurando o Gogs em um servidor Linux

## Atualizando o Linux:
```bash
sudo apt update
sudo apt upgrade -y
```

## Instalando o Go
```bash
sudo apt install golang-go
```

## Configurando o PostgreSQL
```bash
sudo -u postgres psql
postgres=# CREATE DATABASE gogs;
postgres=# CREATE USER gogs WITH PASSWORD 'gogspassword';
postgres=# GRANT ALL PRIVILEGES ON DATABASE gogs TO gogs;
postgres=# \q
```

## Instalando e configurando o Gogs
```bash
# Baixe o Gogs a partir do repositório oficial
curl -s https://api.github.com/repos/gogs/gogs/releases/latest | grep browser_download_url | grep '\linux_amd64.tar.gz' | cut -d '"' -f 4 | wget -i -
# Descompacte o arquivo
tar xvf gogs_*_linux_amd64.tar.gz
# Crie o usuário git
sudo adduser git
# Crie um diretório dedicado para logs
sudo mkdir /var/log/gogs
# Conceda permissão ao diretório para o usuário git
sudo chown -R git:git /var/log/gogs/
# Adicione o arquivo de serviço do Gogs no diretório de sistema
sudo cp gogs/scripts/systemd/gogs.service /etc/systemd/system
# Crie um arquivo de configuração para o Gogs
sudo nano /etc/systemd/system/gogs.service
```

Edite o arquivo para que o Gogs funcione na porta 3001:

	ExecStart=/home/git/gogs/gogs web -port 3001

```bash
# Movimente os arquivos binários do Gogs para a pasta /home/git 
sudo mv gogs /home/git/ 
# Mude as permissões da pasta
sudo chown -R git:git /home/git/
# Inicie o serviço do Gogs
sudo systemctl daemon-reload
sudo systemctl start gogs
# Habilita o serviço do Gogs para iniciar junto com o boot e checa o status
sudo systemctl enable gogs
sudo systemctl status gogs
```

Após as ações acima o Gogs estará acessível no endereço: localhost:3001.
Lembre-se de configurar o usuário administrador.

Caso receba uma mensagem de erro do PostgreSQL, verifique se o usuário gogs é owner do banco de dados gogs.

## Trabalhando com chaves SSH

É extremamente recomendável gerar e instalar uma chave SSH na sua conta para realizar a conexão com o Gogs.

1. Verificando chaves SSH existentes:
```bash
ls -al ~/.ssh
```
2. Abrindo o conteúdo da chave SSH:
```bash
cat ~/.ssh/nome_do_arquivo.pub
```

Caso não tenha nenhuma chave SSH na sua máquina será necessário criar uma.

## Troubleshooting

No meu caso ocorreu um cenário onde desinstalei o PostgreSQL instalado localmente na minha máquina e instalei uma imagem do Docker com PostgreSQL. Com isso, perdi o banco de configuração do Gogs.

Segui os passos abaixo para resolver:

1. Primeiro foi preciso descobrir onde o arquivo de configuração do Gogs estava instalado:
```bash
sudo find / -name app.ini 2>/dev/null
```
2. Depois disso, tive que elevar mihas permissões de usuário para ROOT:
```bash
sudo su -
```
3. Depois foi necessário acessar o arquivo app.ini
```bash
nano /home/git/gogs/custom/conf/app.ini
```
4. Alterar o parâmetro INSTALL_LOCK = true para INSTALL_LOCK = false
5. Reiniciar o serviço do Gogs
```bash
sudo systemctl restart gogs
```

Após este procedimento foi preciso reconfigurar o Gogs com o novo banco de dados. Todos os repositórios foram perdidos, pois eles não existem no novo banco. Mas a pasta com o conteúdo ainda existe na pasta do Gogs.

É possível acessar estas pastas e fazer backup acessando o caminho da instalação do Gogs. No meu caso era: /home/git/gogs-repositories. Nesta pasta você irá encontrar as pastas com as organizações e as pastas com os nomes de usuário e seus respectivos repositórios.

