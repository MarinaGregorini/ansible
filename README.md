Para utilizar o hook post-commit deve usar o comando abaixo para copiar o ficheiro para o diretório correto, e deve alterar as suas permissões de execução:

cp git-hooks/post-commit .git/hooks/post-commit
chmod +x .git/hooks/post-commit

Comando ad-hoc para remover utilizadores:

ansible all -m ansible.builtin.user -a "name={{ item }} state=absent remove=yes" -e "users=['mnunes', 'rmarques', 'csobral']" --become