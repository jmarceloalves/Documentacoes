# PostgreSQL

**Observação:** Os itens 1 a 4 não são etapas obrigatórias para instalar o PostgreSQL. Eu utilizei estes passos pois estava usando o Oracle Virtual Machine na minha máquina pessoal. Caso você esteja instalando o PostgreSQL direto em um servidor, neste caso pode pular direto para o item 4.

## 1 - Atualizar o Ubuntu
```bash
sudo apt update
sudo apt upgrade -y
```
## 2 - Instalar o SSH
```bash
sudo apt-get install openssh-server
```

## 3 - Habilitar o firewall
```bash
sudo ufw enable # Verifica o status do firewall
sudo ufw allow 22/tcp # Habilita a porta 22 (SSH) no servidor
```

**Observação:** Caso esteja usando o Virtual Box, será preciso alterar a placa de rede para bridge. Após configurar a placa, verificar o IP do servidor com o comando "ip addr" (no meu caso: 192.168.15.24). Só então a máquina estará disponível para ser acessada a partir do hospedeiro

```bash
ssh administrator@192.168.15.24
```
## 4 - Começando a instalação do PostgreSQL

### 4.1. Atualizar o Ubuntu

```bash
sudo apt update
sudo apt upgrade -y
```

### 4.2. Adicione o repositório PGDG
```bash
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
```

### 4.3. Importe as chaves de assinatura do repositório
```bash
curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
```

### 4.4. Atualize os índices de pacotes novamente
```bash
sudo apt update
```

### 4.5. Instale o PostgreSQL versão 18
```bash.
sudo apt -y install postgresql-18
```

### 4.6. Iniciar, ativar e verificar o status
```bash
sudo systemctl start postgresql # Iniciar o serviço
sudo systemctl enable postgresql # Ativar o serviço
sudo systemctl status postgresql # Verificar o status do serviço
```
### 4.7. Proteger o banco de dados PostgreSQL no Ubuntu (máquina local)
```bash
sudo -u postgres psql # Faça login como o usuário postgres
ALTER USER postgres WITH ENCRYPTED PASSWORD 'strong_password'; # Altere a senha do usuário postgres
\q # Volte para o usuário logado no Ubuntu

sudo sed -i '/^local/s/peer/scram-sha-256/' /etc/postgresql/18/main/pg_hba.conf # Edite o arquivo de configuração principal para habilitar a autenticação por senha

sudo systemctl restart postgresql # Reinicie o serviço do PostgreSQL para aplicar as alterações
sudo ufw allow 5432 # Habilite a porta 5432 no firewall do servidor
```
### 5. TROUBLESHOOTING

**Observação:** Caso esteja utilizando o Virtual Box com placa de rede no modo Bridge e o servidor estiver indisponível, siga os passos abaixo

### 5.1. Alterar o arquivo postgresql.conf localizado em /etc/postgresql/18/main/
```bash
sudo nano /etc/postgresql/18/main/postgresql.conf # Alterar de [listen_addresses = 'localhost'] para [listen_addresses = '*']
```

### 5.2. Alterar o arquivo pg_hba.conf localizado em /etc/postgresql/18/main/
```bash
sudo nano /etc/postgresql/18/main/pg_hba.conf
```

### 5.3. Realizar as alterações abaixo no arquivo pg_hba.conf
```bash
# IPv4 local connections:
host    all             all             0.0.0.0/0               scram-sha-256
# IPv6 local connections:
host    all             all             ::0/0                   scram-sha-256
```

### 5.4. Reiniciar o serviço do PostgreSQL
```bash
sudo systemctl restart postgresql
```

**Observação:** Após estas alterações o PostgreSQL estará acessível a partir do hospedeiro pelo IP da máquina Virtual
