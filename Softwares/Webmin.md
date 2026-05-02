# Instalar o webmim (https://webmin.com/download/)
```bash
curl -o webmin-setup-repo.sh https://raw.githubusercontent.com/webmin/webmin/master/webmin-setup-repo.sh
sudo sh webmin-setup-repo.sh
sudo apt-get install webmin --install-recommends
sudo ufw allow 10000/tcp # Habilita a porta 10000 (Webmin) no servidor
```

**Observação:** A instância do Webmin ficará disponível no endereço http://localhost:10000
