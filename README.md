> NOTE: Esse é um repositório criado para resolver o erro no Ansible "Missing Sudo Password".

<p align="center">
   <br/>
   <h1 align="center">Resolvendo Missing Sudo Password</h1>
   <p align="center">
   Tutorial de como resolver erro de Missing Sudo Password no Ansible.
   </p>
   <p align="center" style="align: center;">
        <img alt="Ansible" src="https://img.shields.io/badge/Ansible-000000?style=for-the-badge&logo=Ansible&logoColor=white">
   </p>
</p>

## Visão Geral
Ansible é uma ferramenta de TI de código aberto para gerenciar, automatizar, configurar servidores e, implantar aplicativos, a partir de uma localização central. Ele inclui sua própria linguagem declarativa para descrever a configuração do sistema.

## Projeto
A solução de problemas do Ansible, especificamente sobre a falta da senha do sudo e a senha incorreta do sudo.

### Estruturas
* Execução de erro
* Solucionar problemas
* Verificação
* Correção

## Como iniciar
### 1. Clone o repositório
Vamos clonar o repositório.

```
git clone https://github.com/gustavokennedy/resolvendo-missing-sudo-password-ansible.git
cd resolvendo-missing-sudo-password-ansible
ansible-playbook testandosenhasudo.yml
```

Ao digite o comando ansible-playbook acima, você deve receber algo como:

```
PLAY [debugando erro de senha sudo] **********************************************************************

TASK [Gathering Facts] ***********************************************************************************
fatal: []: FAILED! => {"msg": "Missing sudo password"}

PLAY RECAP ***********************************************************************************************
exemplo           : ok=0    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
```
Percebemos que é informado o erro de senha sudo ausente.

### Iniciar a correção

Corrigindo o erro, primeiro você pode tentar acessar via SSH, para realmente verificar se a senha esta correta, provável que terá erro.

```
ssh [usuario]@[ip]
```

Agora vamos alterar alterar as permissões do usuário. Acessamos o servidor com root e alteramos os previlégios do seu usuário:

```
su -
ls -al /etc/sudo
ls -al /etc/sudoers.d/
```
Altere o seguinte arquivo, trocando [usuario] pelo seu usuário do servidor.
Insira a seguinte linha:

usuario ALL=(ALL) NOPASSWD: ALL

```
vim /etc/sudoers.d/usuario
```

Agora deslogue do root e volte para seu usuário, garantá que está como usuário não root.

```
whoami
```

Agora tente conectar com root para verificar as permissões:

```
sudo su
```
Se você conseguiu acesso ao root, está OK.

Em resumo precisamos dar o previlégio ao usuário no /etc/sudoers.d/usuario.

### Correção
Vamos executar o playbook novamente e testar:

```
ansible-playbook testandosenhasudo.yml
```

A saída deve ser:

```
PLAY [debugando erro de senha sudo] **********************************************************************

TASK [Gathering Facts] ***********************************************************************************
ok: [exemplo]

TASK [root test] *****************************************************************************************
ok: [exemplo] => {
"msg": "Previlégios estão OK"
}

PLAY RECAP ***********************************************************************************************
exemplo           : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
