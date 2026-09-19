---
layout: single
title: "Como automontar um diretório em rede no Linux"
date: 2026-09-18
read_time: true
share: true
related: true
classes: wide
author_profile: true
---

Esse tutorial irá mostrar como ativar a montagem automática de um diretório em rede no Linux utilizando a ferramenta "sshfs". Isso irá
permitir que o seu sistema operacional e todos os apps rodando nele possam interagir com a árvore de diretórios do sistema remoto
como se fizesse parte do sistema local.

Vou utilizar o Debian 13 como base para este tutorial. Se você utiliza outra distro, precisará adaptar os procedimentos de acordo.

Para que esse procedimento funciona, o pacote "openssh-server" deverá estar instalado, e o
serviço "sshd" deverá estar habilitado e rodando no PC remoto. Você pode verificar o status com o comando abaixo:

```
systemctl status sshd
```

## Instalando o SSHFS

Primeiro vamos instalar o "sshfs", a ferramenta que faz tudo isso acontecer.

```
sudo apt install sshfs
```

## Criando o script de automação

Agora, crie (se não existir) a pasta na qual o nosso script precisará ficar para ser reconhecido pelo sistema:

```
mkdir -p "$HOME/.local/bin"
```

Copie e cole o código abaixo em um editor de texto e salve-o com o nome `sftp_automount.service` na pasta que você acabou de criar.

**ATENÇÃO:** Substitua os valores das 4 vaiáveis de acordo com as informações correspondentes para o seu caso de uso. Em caso de
dúvidas, leia atenciosamente os comentários do script abaixo.

```
#!/bin/bash

# Variáveis:

userip="username@192.168.100.1" 		          # Usuário e IP do dispositivo remoto.
mymountpoint="$HOME/.mnt/NAS" 			          # Diretório no PC local onde o sistema de arquivos remoto será montado.
mymountsource="/" 				          # Diretório do dispositivo remoto que será montado no PC local.
errorfilelocation="$HOME/sftp_did_not_mount"         	  # Localização do indicador de erro no PC local.

# Execução:

sleep 10 # Os "sleeps" serve para dar tempo do Tailscale iniciar, caso esteja sendo utilizado.

mkdir -p "$mymountpoint"

sshfs $userip:$mymountsource $mymountpoint -o reconnect,ServerAliveInterval=15,ServerAliveCountMax=120 -f
if [[ $? != 0 ]]; then
    sleep 20
    sshfs $userip:$mymountsource $mymountpoint -o reconnect,ServerAliveInterval=15,ServerAliveCountMax=120 -f || touch "$errorfilelocation"
fi
```

## Criando e ativando o serviço no systemd

Por fim, crie um serviço do systemd para que o script rode automaticamente toda vez que o PC iniciar. Para isso, cole
o conteúdo abaixo em um editor de textos:

```
[Unit]
Description=Monitor de conexão e reboot automático

[Service]
ExecStart=%h/.local/bin/sftp_automount

[Install]
WantedBy=default.target
```

Crie a pasta na qual o arquivo será salvo:

```
mkdir -p "$HOME/.config/systemd/user"
```

Agora, salve o arquivo na pasta que você acabou de criar com o nome `sftp_automount.service` e rode
o comando abaixo para ativar e habilitar o serviço:

```
systemctl --user enable --now sftp_automount.service
```

Reinicie o PC e pronto. O sistema de arquivos do dispositivo remoto devera estar montado na pasta que você escolheu.

Agora, basta adicionar essa pasta como um atalho/favorito na barra lateral do gerenciador de arquivos e você terá acesso
automático e facilitado à toda a árvore de diretórios do PC remoto.
