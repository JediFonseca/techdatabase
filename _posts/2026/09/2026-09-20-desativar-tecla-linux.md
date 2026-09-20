---
layout: single
title: "Como desabilitar teclas específicas do teclado no Linux"
date: 2026-09-20
read_time: true
share: true
related: true
classes: wide
author_profile: true
categories: [Linux]
header:
  teaser: /assets/images/thumbnails/2026-09-20-desativar-tecla-linux.jpg
---

Tutorial rápido mostrando como desativar uma ou mais teclas do teclado do PC ou laptop no Linux.

Utilizei o “keyd” no Zorin OS, que é baseado no Ubuntu, por isso os comandos de instalação de pacotes que utilizei são provenientes do mesmo. Lembre-se de adaptar esses comandos para o gerenciador de pacotes da sua distro.

## Instalando dependências

Instale alguns pacotes necessarios para a instalação do “keyd”:

```
sudo apt install git make nano gcc
```

## Instalando o "keyd"

Instale o “keyd” rodando os comandos a seguir (teoricamente válidos para qualquer distro):

```
git clone https://github.com/rvaiya/keyd
```

```
cd keyd
```

```
make && sudo make install
```

```
sudo systemctl enable --now keyd
```

## Configurando e utilizando o Keyd

Crie um arquivo de configurações para o “keyd” com o comando:

```
sudo nano /etc/keyd/default.conf
```

O comando acima terá iniciado, no terminal, o editor de texto “nano” com um arquivo em branco. Dentro deste arquivo, utilizando o atalho “Ctrl + Shift + V” cole o conteúdo a seguir:

```
[ids]

*

[main]

# Maps capslock to escape when pressed and control when held.
#capslock = overload(control, esc)

# Remaps the escape key to capslock
#esc = capslock
```

No final deste arquivo de texto escreva o nome da tecla que você deseja desabilitar, seguido pelo comando “noop”. Por exemplo, para desabilitar a tecla “Insert” escreva:

```
insert = noop
```

Para descobrir o nome/identificador da tecla que você deseja desabilitar rode o comando `sudo keyd monitor` e pressione a tecla desejada. Isso exibirá na tela do terminal qual é o identificador de cada tecla pressionada.

Para salvar o arquivo de texto através do nano pressione “Ctrl + O” seguido de “Enter” e “Ctrl + X” para fechar o editor.

Para aplicar todas as mudanças feitas até agora, rode o comando:

```
sudo keyd reload
```

A tecla escolhida já não deve estar funcionando.

## Revertendo as mudanças

Para reverter as mudanças basta que você comente ou remova a linha correspondente à tecla desejada do arquivo de configurações do “keyd” e rode o comando de “reload” novamente.

## Outras funções do “keyd”

O “keyd” serve para outras funções relacionadas ao remapeamento de teclas, não apenas a desabilitação de teclas específicas. Você pode, por exemplo, 
fazer com que uma tecla passe a operar como se fosse outra, dentre outras coisas.

Para mais informações visite a página do software no [**Github**](https://github.com/rvaiya/keyd).
