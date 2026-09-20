---
layout: single
title: "Resolvendo problemas com o VirtualBox "
date: 2026-09-20
read_time: true
share: true
related: true
classes: wide
author_profile: true
categories: [Linux, Windows, Redes]
header:
  teaser: /assets/images/thumbnails/2026-09-20-corrigindo-erros-virtualbox.jpg
---

Nesse tutorial vamos resolver dois problemas que podem impedir o VirtualBox de funcionar.

O tutorial é baseado no Debian 13, mas deve funcionar também em todos os derivados do Debian e do Ubuntu. 
Porém, para distros de outras bases será necessário adaptar o comando do gerenciador de pacotes (de apt para pacman, dnf e assim por diante…). 
Os nomes dos pacotes também podem precisar de ajustes, já que nem sempre são os mesmos em diferentes distros.

## Problema 01: O erro do VirtualBox após atualizações de Kernel

Ocasionalmente o VirtualBox pode deixar de funcionar após alterações no Kernel. Recentemente, instalei o Kernel Liquorix 
para fazer alguns testes. Depois disso, instalei e usei o VirtualBox. Então, decidi voltar para o Kernel padrão do Debian 13 e o VirtualBox parou de funcionar.

O erro que apareceu após ter feito modificações no kernel foi:

![erro-do-virtualbox]({{ site.baseurl }}/assets/images/body/2026-09-20-virtualbox-01a.png)

Que apareceu ao mesmo tempo que o seguinte:

![erro-do-virtualbox2]({{ site.baseurl }}/assets/images/body/2026-09-20-virtualbox-01b.png)

### A solução

Primeiro, instale os pacotes necessários para que o VirtualBox consiga “gerar” os seus drivers:

```
sudo apt update && sudo apt install build-essential linux-headers-amd64
```

Agora que o sistema possui as ferramentas necessárias, reconfigure o VirtualBox:

```
sudo /sbin/vboxconfig
```

Agora reinicie e teste se tudo está funcionando.

## Problema 02: O conflito com o KVM

Em alguns casos o VirtualBox pode não iniciar as VMs porque o sistema de virtualização do CPU está sendo utilizado 
pelo KVM (uma outra ferramenta de virtualização). Esse erro ocorre porque apenas um hipervisor pode controlar as extensões 
de virtualização do hardware por vez. Então se o KVM estiver controlando essas extenções, o VirtualBox não consegue. 
Nesse caso, o erro abaixo será exibido:

![erro-do-virtualbox3]({{ site.baseurl }}/assets/images/body/2026-09-20-virtualbox-02.png)

Se o seu PC tiver um processador da Intel, o erro irá mostrar “kvm_intel”. No print está mostrando “kvm_amd” porque o meu CPU é da AMD.

> **ATENÇÃO:** Os procedimentos abaixo irão adicionar o KVM à blacklist durante a inicialização do sistema. Isso tem um efeito colateral 
que pode ser relevante para algumas pessoas, pois outros softwares de virtualização como o GNOME Boxes, Virt-Manager e semelhantes irão parar de funcionar.

Se você precisar desses softwares funcionando junto com o VirtualBox, apenas rode o comando `sudo modprobe -r kvm_amd` para PC’s com CPU da AMD 
ou `sudo modprobe -r kvm_intel` para PC’s com CPU da Intel. Isso irá parar o KVM temporariamente, até a próxima reinicialização e permitirá o uso do VirtualBox enquanto isso.

Se você não pretende utilizar o GNOME Boxes, o Virt-Manager e semelhantes, pode manter o KVM pausado permanentemente com os procedimentos à seguir.

### Como pausar o KVM permanentemente em PC’s com CPU’s da AMD

Execute os três comandos abaixo, na ordem em que aparecem e reinicie o sistema.

```
sudo modprobe -r kvm_amd
```

```
echo -e "blacklist kvm\nblacklist kvm_amd" | sudo tee /etc/modprobe.d/blacklist-kvm.conf
```

```
sudo update-initramfs -u
```

Agora o VirtualBox deve estar funcionando perfeitamente no seu PC AMD.

### Como pausar o KVM permanentemente em PC’s com CPU’s da Intel

Execute os três comandos abaixo, na ordem em que aparecem e reinicie o sistema.

```
sudo modprobe -r kvm_intel
```

```
echo -e "blacklist kvm\nblacklist kvm_intel" | sudo tee /etc/modprobe.d/blacklist-kvm.conf
```

```
sudo update-initramfs -u
```

Agora o VirtualBox deve estar funcionando perfeitamente no seu PC Intel.

## Como desfazer o procedimento de pausa do KVM

Se em algum momento você quiser desfazer o procedimento anterior para poder voltar a usar softwares como o GNOME Boxes e o Virt-Manager, apenas rode os comandos abaixo no seu terminal:

```
sudo rm /etc/modprobe.d/blacklist-kvm.conf
```

```
sudo update-initramfs -u
```

Reinicie e pronto! O seu KVM já voltou a funcionar.
