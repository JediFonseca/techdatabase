---
layout: single
title: "Ajustando a largura dos botões de janela no painel do Cinnamon"
date: 2026-09-16
read_time: true
share: true
related: true
classes: wide
author_profile: true
categories: [Linux, Personalização]
---

O Cinnamon é o desktop environment (popularmente chamado de "interface gráfica") padrão e originária do Linux Mint - embora também seja utilizada em outras distros.
A altura do painel (equivalente a Barra de Tarefas do Windows) pode ser facilmente ajustada pelas configurações visuais do sistema, mas não há uma
opção nativa para configurar a largura dos botões de janela que ficam nesse painel.

À medida que o painel é deixado mais alto, os botões ficam proporcionalmente mais estreitos, o que pode incomodar algumas pessoas.

Nesse artigo, vamos ver como alterar a largura desses botões para qualquer tamanho modificando um arquivo de configurações do tema em uso.

## Fazendo as modificações

01 - Abra a seção de Temas nas configurações do sistema e verifique qual é o nome do tema em uso.

02 - Encontre a pasta do tema em uso, que normalmente fica em `/usr/share/themes` ou `~/.local/share/themes` para temas instalados pelo usuário.

03 - Dentro da pasta do tema em uso, abra o arquivo chamado `cinnamon.css`. Você precisará ser root/usar `sudo` para modificar arquivos em 
`/usr/share/themes`. Recomendo utilizar o editor de textos `xed`, padrão do Linux Mint, executado como `root` rodando `sudo xed` no terminal.

04 - Dentro do `cinnamon.css`, procure pela sessão `.grouped-window-list-item-box`.

05 - Abaixo da sessão mencionada acima será possível observar items como `text-align`, `font-weight`, `background-image`, entre outros.
Adicione `min-width: 50px;` em uma nova linha e salve o arquivo. Substitua `50px` pela largura (em pixels) que você deseja que os botões tenham.

06 - Pressione `Alt + F2`, digite a letra `r` e pressione `Enter` para reiniciar o cinnamon. Isso deverá aplicar as modificações.

Continue alterando o valor de `min-width: 50px;` e repetindo o passo 6 até alcançar o resultado desejado.
