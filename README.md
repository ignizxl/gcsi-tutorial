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