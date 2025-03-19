Após a criação do hook post-commit, fizemos:

chmod +x .git/hooks/post-commit

Comando ad-hoc para remover utilizadores:

ansible all -m ansible.builtin.user -a "name={{ item }} state=absent remove=yes" -e "users=['mnunes', 'rmarques', 'csobral']" --become