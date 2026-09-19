---
layout: single
title: "Como acessar o Windows 11 à partir do Linux via SSH"
date: 2026-09-16
read_time: true
share: true
related: true
classes: wide
author_profile: true
---

Esse tutorial irá mostrar como acessar o PowerShell do Windows 11 à partir de uma distro Linux ou do Android utilizando SSH.

## Instalar e configurar o OpenSSH Server

01 - Abra o PowerShell como administrador e verifique se o "OpenSSH Server" aparece como "Installed":

```
Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'
```

02 - Se não estiver instalado, instale-o com o comando abaixo:

```
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

03 - Inicie o serviço SSHD:

```
Start-Service sshd
```

04 - Configure o SSHD para que inicie automaticamente com o Windows:

```
Set-Service -Name sshd -StartupType 'Automatic'
```

05 - O SSH irá utilizar a porta 22 para se comunicar com os outros dispositivos. Libere essa porta no Firewall do Windows:

```
New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH SSH Server' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
```

06 - Acesse o PC Windows via SSH de qualquer outro dispositivo da rede local com:

```
ssh usuario@IP_DO_WINDOWS
```

### Passo opcional

Se você quiser fazer login no Windows automaticamente (sem pedir senha) e ao mesmo tempo manter a senha do usuário que possibilida
o uso so SSH, siga esses 3 passos.

01 - Rode o comando abaixo para alterar a chave do Windows Auto Logon no registro:

```
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v AutoAdminLogon /t REG_SZ /d 1 /f
```

02 - Agora, rode o comando a seguir substituindo "NomeDoUsuário" pelo seu nome de usuário:

```
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v DefaultUsername /t REG_SZ /d "NomeDoUsuario" /f
```

03 - Por fim, rode o comando abaixo substituindo "SuaSenhaAqui" pela sua senha:

**ATENÇÃO:** A senha fica salva em texto simples no registro. Faça por sua conta e risco.

```
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v DefaultPassword /t REG_SZ /d "SuaSenhaAqui" /f
```

#### Revertendo todas essas mudanças:

Desfaça o passo 01:

```
reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v AutoAdminLogon /t REG_SZ /d 0 /f
```

Desfaça o passo 02:

```
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v DefaultPassword /f
```

Desfaça o passo 03:

```
reg delete "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v DefaultUsername /f
```

### IMPORTANTE

- Você precisará de uma senha configurada para o seu usuário do Windows para poder acessá-lo via SSH.
- Para saber qual é o nome de usuário correto para acessar o Windows via SSH, abra o PowerShell e verifique qual nome aparece no prompt. Esse é o seu
nome de usuário. Se esse nome contiver espaços, utilize aspas no comando de acesso do ssh. Exemplo: `ssh "nome sobrenome@192.168.100.1"`.
