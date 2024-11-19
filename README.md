<h1 align="center">Configuração de redirecionamento no Nginx e Teste de carga com Apache Bench</h1>

> ⚠️ AVISO: Todos os comandos utilizados neste repositório foram realizados em uma distribuição Linux Ubuntu.


### Etapa 1 – Instalando o Nginx

>      $ sudo apt update
>      $ sudo apt install nginx -y

### Etapa 2 – Ajustando o Firewall

Primeiro, ative o firewall com o comando:
    
>      $ sudo ufw enable

  ![ufw_enable](assets/ufw_enable.png)

Liste as configurações do aplicativo utilzando o comando:

>      $ sudo ufw app list

Você deve obter uma lista dos perfis de aplicativos:

  ![ufw_app_list](assets/ufw_app_list.png)

- Nginx Full : Este perfil abre a porta 80 (tráfego da web normal e não criptografado) e a porta 443 (tráfego criptografado TLS/SSL)

- Nginx HTTP : Este perfil abre apenas a porta 80 (tráfego da web normal e não criptografado)

- Nginx HTTPS : Este perfil abre apenas a porta 443 (tráfego criptografado TLS/SSL)

> ⚠️ AVISO: É recomendado que você habilite o perfil mais restritivo que ainda permitirá o tráfego que você configurou. Mas no momento, só precisaremos permitir tráfego na porta 80.
Você pode habilitar isso digitando:

>      $ sudo ufw allow 'Nginx HTTP'

  ![ufw_allow](assets/ufw_allow.png)

Você pode verificar a alteração digitando:

>      $ sudo ufw status

  ![ufw_status](assets/ufw_status.png)

  ### Etapa 3 – Configurando o seu projeto
Crie um diretório chamado "gcsi2024" no **/home/<usuário>** e entre no diretório criado digitando:

>      $ mkdir gcsi2024 && cd gsci2024

Clone o projeto cafeteria-web dentro do diretório "gcsi2024":

>      $ git clone https://github.com/rhavymaia/cafeteria-web.git

   ou, clone via SSH

>     $ git clone git@github.com:rhavymaia/cafeteria-web.git

  ![mkdir_e_gitclone](assets/mkdir_e_gitclone.png)

Instale as dependências do projeto digitando:

>      $ npm i

Compile o projeto digitando: 

>      $ npm run build

### Etapa 4 – Configuração de redirecionamento no Nginx

Dentro do projeto, navegue até o diretório **dist/** e utilize o comando pwd para retornar o path completo, para isso, utilize os comandos:

>     $ ls
>     $ cd dist/
>     $ pwd

  ![pwd](assets/pwd.png)

Crie um arquivo .conf dentro do diretório **/etc/nginx/sites-available/** digitando:

>     $ cd /etc/nginx/sites-available/
>     $ sudo nano cafeteria.conf

  ![sudo_nano_cafeteria](assets/sudo_nano_cafeteria_conf.png)

Adicione as seguintes configurações no arquivo cafeteria.conf:

  ![nano_cafeteria](assets/nano_cafeteria_conf.png)

> ⚠️ AVISO: Antes de sair do nano certifique-se de salvar as alterações feitas! Utilize **Ctrol + o** para salvar as alterações e **Ctrol + x** para sair.

Navegue até o diretório **/etc/nginx/** e altere o usuário **“user www-data”** para **“user <usuário>”**, para isso, utilize:
	    
>     $ ls
>     $ sudo nano nginx.conf

  ![sudo_nano_nginx_conf](assets/sudo_nano_nginx_conf.png)

Alterando **“user www-data”** para **“user igor”**:

  ![nano_nginx_conf](assets/nano_nginx_conf.png)

Navegue até **/etc/nginx/sites-enabled** e crie um link simbólico no diretório atual, apontando para o arquivo **cafeteria.conf**, localizado no diretório **../sites-available/** e remova o arquivo default, para isso, utilize os seguintes comandos:

>     $ cd /etc/nginx/sites-enabled
>     $ ls
>     $ sudo ln -s ../sites-available/cafeteria.conf .
>     $ ls
>     $ sudo rm default
>     $ ls
    
> ⚠️ AVISO: Certifique-se de utilizar a permisão de super usuário para remover o **default**, caso o contrário a permissão será negada como no exemplo abaixo!

  ![symlink](assets/symlink.png)

Reinicie o Nginx, para isso utilize os seguintes comandos:

>     $ sudo systemctl restart nginx

ou, se o comando **restart** não funcionar, tente estes: 

>     $ sudo systemctl stop nginx
>     $ sudo systemctl status nginx
>     $ sudo systemctl start nginx

  ![systemclt](assets/systemctl.png)

Se todos os passos foram seguidos corretamente até aqui, ao abrir o **localhost** no seu browser deve aparecer a seguinte aplicação:

  ![localhost](assets/localhost.png)