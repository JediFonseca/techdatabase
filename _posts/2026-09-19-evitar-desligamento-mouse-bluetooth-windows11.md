---
layout: single
title: "Como evitar a desativação não comandada de um dispositivo bluetooth no Windows 11"
date: 2026-09-19
read_time: true
share: true
related: true
classes: wide
author_profile: true
---

Esse procedimento foi testado apenas com um mouse bluetooth que estava sendo desativado de forma automatica após poucos minutos
de uso no Windows 11, mas - teoricamente - pode funcionar também com dispositivos bluetooth de outra natureza.

## Desative a economia de energia do Bluetooth

1. Win + X → Gerenciador de Dispositivos
2. Expanda Bluetooth
3. Abra as propriedades do adaptador Bluetooth (não do mouse).
4. Aba Gerenciamento de Energia.
5. Desmarque “Permitir que o computador desligue este dispositivo para economizar energia”.

## Desative temporariamente a suspensão seletiva de USB

Isso é especialmente importante se seu Bluetooth for integrado por USB internamente ou se você estiver usando um dongle.

1. Painel de Controle → Opções de Energia
2. Plano atual → Alterar configurações do plano
3. Alterar configurações de energia avançadas
4. Configurações USB → Configuração de suspensão seletiva de USB
5. Coloque como Desabilitado.

## Desparear, reiniciar e reparear

1. Despareie o dispositivo
2. Reinicie o Windows
3. Pareie o dispositivo novamente

Pronto! Agora é só testar e - se tudo tiver decorrido conforme o esperado - as desconexões não comandadas deverão parar de acontecer.
