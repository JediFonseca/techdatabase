---
layout: single
title: "Como definir o melhor perfil de energia no Linux"
date: 2026-09-17
read_time: true
share: true
related: true
classes: wide
author_profile: true
categories: [Linux]
---

A configuração do CPU Governor no Linux é - de forma simplificada - o equivalente aos perfis de energia do Windows (Economia de energia,
Alto Desempenho, Equilibrado e etc.).

Nesse tutorial, vou mostrar como selecionar um modo específico, à sua escolha, de forma persistente no Linux.

Como existem muitas distros Linux, utilizarei o Debian 13 como exemplo. Se você utiliza outras distros, precisará adaptar os nomes e
o gerenciador de pacotes. Para isso você pode, por exemplo, copiar um comando desse tutorial e perguntar ao Google, ou a alguma IA,
qual é o equivalente desse comando para a sua distro.

## Selecionando o modo do CPU Governor

### 1 - Instalando o "cpupower"

Primeiro, verifique se o `cpupower` já está instalado no seu sistema:

```
command -v cpupower
```

Se o comando acima retornar um caminho como `/usr/bin/cpupower`, significa que já está instalado. Nesse caso, pule para o passo 2. Se
o comando não retornar nada, então instale o `cpupower`:

```
sudo apt install linux-cpupower
```

### 2 - Rode o CPU Power para identificar quais modos estão disponíveis para o seu hardware:

```
cpupower frequency-info | grep -E "disponíveis|available"
```

O comando acima irá retornar a frase `reguladores do cpufreq disponíveis:` seguida pela lista com os nomes
dos perfis que você pode utilizar. Para o exemplo deste tutorial, utilizaremos `performance`, mas outras opções
como `on-demand` e `powersave` também costumam estar disponíveis.

### 3 - Agora rode o comando abaixo para identificar qual modo está ativo no seu hardware:

```
cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
```

### 4 - Ative o modo desejado (nesse caso, o "performance"):

```
sudo cpupower frequency-set -g performance
```

### 5 - Fazer com que a alteração persista às reinicializações:

Para isso, criaremos um serviço do systemd. Cole todo o comando abaixo em um terminal e pressione `Enter`.

```
cat <<EOF | sudo tee /etc/systemd/system/cpupower-custom.service > /dev/null
[Unit]
Description=CPU Custom Governor
After=cpupower.service

[Service]
Type=oneshot
ExecStart=/usr/bin/cpupower frequency-set -g performance

[Install]
WantedBy=multi-user.target
EOF
```

Depois, ative e execute esse serviço:

```
sudo systemctl enable --now cpupower-custom.service
```

### 6 - Verificação final

Por fim, reinicie a máquina, abra um terminal e cheque se o perfil que você selecionou continuou ativado após a reinicialização:

```
cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
```
