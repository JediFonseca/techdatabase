---
layout: single
title: "Mapeamento de Pasta do Nextcloud via WebDAV no Windows 11"
date: 2026-09-22
read_time: true
share: true
related: true
classes: wide
author_profile: true
categories: [Windows, Redes]
header:
  teaser: /assets/images/thumbnails/2026-09-22-nextcloud-webdav-w11.png
---

Este guia descreve o procedimento para mapear o repositório de arquivos do Nextcloud como uma unidade de rede no Windows 11 utilizando 
a interface gráfica (Windows Explorer), sem necessidade de sincronização local de arquivos.

## Pré-requisitos e Ajustes no Windows

Antes de iniciar o mapeamento na interface gráfica, certifique-se de que o Windows possui os componentes necessários ativos.

### 1. Ativar o serviço Cliente Web (WebClient)
Por padrão, o Windows pode desativar o serviço necessário para conexões WebDAV, ocasionando o *Erro de Sistema 67*.

1. Na busca do menu Iniciar digite **Serviços** e abra o aplicativo de mesmo nome.
2. Localize o serviço **Cliente da Web** (*WebClient*).
3. Altere o **Tipo de inicialização** para **Automático** e clique em **Iniciar**.
4. Clique em **Aplicar** e depois em **OK**.

![serviços-do-windows]({{ site.baseurl }}/assets/images/body/2026-09-22-nextcloud-webdav-w11-1.png)

## Mapeando via Windows Explorer

### Passo 1: Obter a URL WebDAV no Nextcloud

1. Acesse o Nextcloud pelo navegador.
2. No canto inferior esquerdo da tela de **arquivos**, clique em **Configurações de arquivos**.
3. Na aba **WebDAV**, copie o endereço exibido no campo **URL WebDAV**.  
   *Exemplo:* `https://example.com/nextcloud/remote.php/dav/files/USERNAME/`

![serviços-do-windows]({{ site.baseurl }}/assets/images/body/2026-09-22-nextcloud-webdav-w11-2.png)

### Passo 2: Realizar o Mapeamento no Windows

1. Abra o **Windows Explorer** e selecione **Este Computador**.
2. Na barra de ferramentas superior, clique no ícone de três pontos (**...**) e selecione **Mapear unidade de rede**.
3. Na janela que se abrir, escolha uma letra para o **Drive** (ex: **S:**).
4. No campo **Pasta**, cole a **URL WebDAV** copiada do Nextcloud.
5. Marque as opções:
   * **Conectar-se na entrada**
   * **Conectar usando credenciais diferentes**
6. Clique em **Concluir**.

![serviços-do-windows]({{ site.baseurl }}/assets/images/body/2026-09-22-nextcloud-webdav-w11-3.png)

7. Na janela de **Segurança do Windows**, insira seu **Usuário** e **Senha** do Nextcloud (ou uma *Senha de Aplicativo* gerada no painel de segurança do Nextcloud).
8. Marque a opção **Lembrar minhas credenciais**.
9. Clique em **OK**.

![serviços-do-windows]({{ site.baseurl }}/assets/images/body/2026-09-22-nextcloud-webdav-w11-4.png)

## Conclusão

Após a autenticação, a pasta do Nextcloud abrirá automaticamente no Windows Explorer como um novo disco. Arquivos criados ou modificados nessa unidade 
serão salvos e indexados diretamente pelo servidor do Nextcloud em tempo real. Para mais informações acesse a [**documentação oficial**](https://docs.nextcloud.com/server/stable/user_manual/pt_BR/files/access_webdav.html).
