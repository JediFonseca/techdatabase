---
layout: single
title: "Como Acessar um PC Linux via Samba no Windows 11"
date: 2026-09-16
read_time: true
share: true
related: true
classes: wide
---

Este guia passo a passo explica como configurar um compartilhamento de rede utilizando o **Samba** em um servidor Debian e acessá-lo a partir do **Windows 11**. 
Se você estiver utilizando outro sistema Linux, adapte os comandos do gerenciador de pacotes (apt) para a sua distro.

---

## Passo a passo no PC rodando Linux

1. **Atualize o sistema e instale o Samba:**

No PC Linux, rode o comando abaixo (Adaptando o comando para a sua distro):

```
sudo apt update && sudo apt install samba -y
```

2. **Crie uma senha do Samba para o seu usuário do Linux:**

   > **Nota:** Substitua `SEU_USUARIO` pelo seu nome de usuário real no Linux.

```
sudo smbpasswd -a SEU_USUARIO
```

3. **Edite o arquivo de configuração do Samba:**

```
sudo nano /etc/samba/smb.conf
```
  
Adicione o seguinte bloco ao final do arquivo:
```
[nomeasuaescolha]
comment = Comentário à sua escolha
path = "/home/SEU_USUARIO/compartilhado"
browseable = yes
read only = no
valid users = SEU_USUARIO
```

4. No arquivo `smb.conf`, substitua:

- "nomeasuaescolha" por qualquer nome que você queira;
- "Comentário à sua escolha" por qualquer comentário;
- "SEU_USUARIO" pelo seu nome de usuário do PC Linux;
- "/home/SEU_USUARIO/compartilhado" pelo diretório do PC Linux que você deseja compartilhar.

6. **Reinicie e verifique o serviço do Samba:**

```
sudo systemctl restart smbd
sudo systemctl status smbd
```

*Certifique-se de que o status exibe `active (running)`.*

7. No terminal do Debian, execute o comando abaixo para obter o IP local:

```
hostname -I
```

Anote o endereço retornado (exemplo: `192.168.1.100`).

---

## Passo a passo no PC com o Windows 11

1. Abra o **Explorador de Arquivos** no Windows 11.
2. Clique com o botão direito sobre **Este Computador** no menu lateral e selecione Mapear unidade de rede...** (ou clique no ícone de três pontos `...` no menu superior e escolha *Mapear unidade de rede...*).
3. Selecione uma letra de unidade (ex: `Z:`).
4. No campo *Pasta*, digite o caminho completo: `\\192.168.1.100\nomeasuaescolha`. O "nomeasuaescolha" tem que ser o mesmo do `smb.conf`.
5. Marque **Reconectar-se na entrada**, **Conectar usando credenciais diferentes** e clique em **Concluir**.
6. Digite o login e a senha, marque **Lembrar minhas credenciais** e clique em **OK**.

---

## Solução de Problemas Comuns

- **Erro de Acesso Negado / Permissão:**
  Verifique se o usuário do Linux tem permissão de escrita/leitura na pasta compartilhada:
  ```bash
  sudo chown -R SEU_USUARIO:SEU_USUARIO /home/SEU_USUARIO/compartilhado
  ```
- **Firewall Bloqueando a Conexão:**
  Se estiver usando o `ufw` no Linux, libere as portas do Samba:
  ```bash
  sudo ufw allow samba
  ```
  
