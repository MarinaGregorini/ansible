# Projeto Ansible - Automação de Infraestrutura

## Introdução
Este projeto utiliza **Ansible** para a gestão e configuração de um conjunto de máquinas virtuais. As principais funcionalidades incluem:

- Instalação automatizada de pacotes por grupos de servidores.
- Criação e gerenciamento de utilizadores com permissões específicas.
- Deploy automatizado de uma aplicação Python.
- Execução de playbooks via **Git Hooks**.

---

## Configuração Inicial

### Remover entradas antigas do SSH (evitar conflitos de autenticação)
Antes de conectar às VMs, remova quaisquer chaves SSH antigas associadas aos IPs das máquinas virtuais:
```bash
for ip in 10.101.151.181 10.101.151.182 10.101.151.183 10.101.151.184 10.101.151.185 10.101.151.186 10.101.151.187 10.101.151.188 10.101.151.189 10.101.151.190; do    
    ssh-keygen -R $ip
done
```

### Criar diretório e configurar ambiente no WSL
Abra uma nova janela no **VS Code** conectada ao **WSL** e crie o diretório do projeto:
```bash
mkdir ansible
cd ansible
```

### Criar e ativar ambiente virtual
Execute os comandos abaixo para criar e ativar um ambiente virtual Python:
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### Clonar o repositório do GitHub
Conecte o diretório ao repositório remoto e clone-o:
```bash
git remote add origin https://github.com/MarinaGregorini/ansible.git
git clone https://github.com/MarinaGregorini/ansible.git
cd ansible
```

### Instalar dependências necessárias
Antes de executar os playbooks, instale os pacotes requeridos:
```bash
sudo apt update && sudo apt install python3-pip -y
pip install -r requirements.txt
```

---

## Execução do Projeto

### Configurar o Git Hook `post-commit`
Para garantir a execução automática dos playbooks após cada commit, copie e configure o hook:
```bash
cp git-hooks/post-commit .git/hooks/post-commit
chmod +x .git/hooks/post-commit
```
Agora, toda vez que um commit for feito, os playbooks serão executados automaticamente.

---

## Testando as Implementações

### Conectar-se às VMs
Para testar se as configurações foram aplicadas corretamente, conecte-se às VMs via SSH utilizando o usuário **upskill-admin**:
```bash
ssh upskill-admin@10.101.151.xxx
```
Substitua `10.101.151.xxx` por um dos IPs listados no ficheiro `inventory.ini`.

### Verificar pacotes instalados e grupos de utilizadores
Dentro da VM, execute o seguinte comando para validar a instalação dos pacotes e visualizar os grupos de utilizadores:
```bash
echo "sl: $(sl --version)" && \
echo "vim: $(vim --version | head -n 1)" && \
echo "btop: $(btop --version | head -n 1)" && \
echo "iperf3: $(iperf3 --version | head -n 1)" && \
echo "htop: $(htop --version | head -n 1)" && \
echo "fortune: $(fortune --version | head -n 1)" && \
echo "curl: $(curl --version | head -n 1)" && \
echo -e "\nGrupos no sistema:" && \
getent group
```

### Inicializar a aplicação Python
Na VM **10.101.151.190**, execute o comando abaixo para iniciar a aplicação:
```bash
cadeia-logistica
```
A aplicação estará disponível em: [http://10.101.151.190:5000](http://10.101.151.190:5000)

---

## Comando Ad-hoc para Remover Utilizadores
Para remover utilizadores de todas as VMs, utilize o comando ad-hoc abaixo:
```bash
ansible all_servers -m shell -a "for user in mnunes rmarques csobral; do userdel -r \$user; done" --ask-become-pass --ask-pass --become
```
Isso garante que os utilizadores serão removidos completamente das máquinas.

### Testar remoção de utilizadores
Após executar o comando de remoção, entre novamente em uma VM e verifique se os utilizadores foram realmente excluídos:
```bash
ls /home/ && \
getent group
```

---

## Autores
- Bruna Dutra  
- Marina Gregorini  
- Marta Martins  
- Tiago Silva  

## Repositório
[GitHub - MarinaGregorini/ansible](https://github.com/MarinaGregorini/ansible)
