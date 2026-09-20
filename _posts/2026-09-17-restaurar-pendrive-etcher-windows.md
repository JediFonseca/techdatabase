---
layout: single
title: "Restaurar Pen Drive utilizado com o Balena Etcher no Windows 11"
date: 2026-09-17
read_time: true
share: true
related: true
classes: wide
author_profile: true
categories: [windows]
---

Quando o **Balena Etcher** grava uma imagem no pen drive, ele geralmente apaga e recria a tabela de partições, às vezes utilizando formatos não reconhecidos pelo Windows.
Por isso, o Windows pode mostrar o pendrive como “não formatado” ou com espaço reduzido. Para dificultar a situação, o Gerenciador de Partições do Windows normalmente
não é capaz de fazer modificações nesse tipo de dispositivo.

Este guia mostra como restaurar o dispositivo para **FAT32** e/ou **exFAT** no **Windows 11**.

## Restaurando o pen drive

Para restaurar o pen drive, utilizaremos um método que faz uso do Diskpart: uma ferramenta de linha de comando que funciona com o PowerShell.

> ⚠️ **Atenção:** Este processo apaga todos os dados do pen drive.  
> Certifique-se de selecionar o disco correto, pois o comando `clean` apaga tudo!

1 - **Conecte o pen drive** ao PC.

Com o pen drive conectado em uma porta USB, acesse o Menu Iniciar, pesquise por "PowerShell" e abra-o.

2 - Acesse o Diskpart digitando:

```
diskpart
```

3 - Liste todos os dispositivos de armazenamento conectados:

```
list disk
```

> Identifique o seu pen drive pelo **tamanho**.  

4 - Selecione o pen drive (substitua `X` pelo número correto):

```
select disk X
```

5 - Apague a tabela de partições:

```
clean
```

6 - Crie uma nova partição:

```
create partition primary
```

7 - Formate como **FAT32** (rápido):

```
format fs=fat32 quick
```

7.1 - Se o dispositivo for maior do que 32GB ou se preferir, formate em exFAT:

```
format fs=exfat quick
```

8 - Atribua uma letra para que a unidade apareça no Explorer:

```
assign
```

9 - Saia do Diskpart:

```
exit
```

Agora o pen drive estará vazio, em **FAT32/exFAT** e pronto para uso.
