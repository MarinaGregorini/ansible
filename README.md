1- Remover as chaves ssh antigas relacionadas aos IPs das VMs:

for ip in 10.101.151.181 10.101.151.182 10.101.151.183 10.101.151.184 10.101.151.185 10.101.151.186 10.101.151.187 10.101.151.188 10.101.151.189 10.101.151.190; do    
    ssh-keygen -R $ip
done

2- Numa nova janela, conectar o VS Code ao WSL e criar um diretório:

mkdir ansible

3- Criar e ativar o ambiente virtual:

python3 -m venv .venv
source .venv/bin/activate

4- Conectar o diretório ao repósitório remoto do Git Hub e cloná-lo:

git remote add origin https://github.com/MarinaGregorini/ansible.git
git clone https://github.com/MarinaGregorini/ansible.git

5- Instalar as dependências necessárias:

sudo apt install python3-pip
pip install -r requirements.txt

6- Para utilizar o hook post-commit deve usar o comando abaixo para copiar o ficheiro para o diretório correto, e deve alterar as suas permissões de execução:

cp git-hooks/post-commit .git/hooks/post-commit
chmod +x .git/hooks/post-commit

7- Comando ad-hoc para remover utilizadores:

ansible all -m ansible.builtin.user -a "name={{ item }} state=absent remove=yes" -e "users=mnunes,rmarques,csobral" --become