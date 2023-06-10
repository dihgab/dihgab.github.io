---
layout: post
title: "Como manter um processo em execução 24hrs por dia"
date: 2023-06-10 18:30:00 -0300
categories: [Linux, Comandos]
tags: linux comandos systemctl systemd bots rotina
image: "/assets/img/Systemctl-24-7.png"
---

> Bots não dormem. Bots não dão problema. 
> *Mas se algo acontecer, que reiniciem automaticamente*. :v 

# Systemctl

O comando `systemctl` é o gerenciador do `systemd`, que gerencia o Linux. É uma ferramenta essencial para administração de servidores Linux e permite total controle sobre os serviços em execução no sistema. O `systemctl` pode ser usado para controlar um serviço pelos comandos `status`, `start`, `stop` e `restart`.

Um serviço do sistema operacional pode ser usado para manter bots no ar ou para qualquer outra coisa que seja executada sempre ou em intervalos bem definidos, a qual possui dois serviços.

#### 1. O bot que fala com as pessoas

Existe um serviço principal que é responsável por responder as pessoas, coletar os códigos de pacotes enviados e também responder a comandos, como `/start`, `/gif` etc. Este serviço não procura por atualizações nos pacotes. Ele apenas consulta o banco de dados. Não há qualquer consulta ao sistema dos Correios neste serviço.

#### 2. A rotina que verifica os pacotes

O segundo serviço é a rotina que verifica os pacotes. Esta rotina é executada usando-se um serviço do sistema operacional e é executada em intervalos definidos. Isto é, espera-se o tempo escolhido(`RestartSec`) antes de executar a rotina novamente. A vantagem desta abordagem em relação ao uso de um *cronjob* é que evita que uma rotina seja iniciada em paralelo à anterior caso ela não tenha terminado. Ou seja, não importa a duração de tempo para que o arquivo python seja executado. O tempo entre as rotinas é sempre o mesmo e não há execuções em paralelo.

## Criar um serviço

Crie um arquivo chamado *\<nome do serviço\>.service* na pasta `/lib/systemd/system` com o conteúdo abaixo:

```shell
# cd /lib/systemd/system
# nano <nome do serviço>.service
```

```
[Unit]
Description=<DESCRIÇÃO>
After=multi-user.target

[Service]
Type=simple
WorkingDirectory=<DIRETÓRIO>
ExecStart=<COMANDO>
Restart=always
RestartSec=<INTERVALO>

[Install]
WantedBy=multi-user.target
```

Defina os valores conforme sua preferência.

- `Description`: Descrição do serviço; 
- `After`: Em que momento o serviço será iniciado;
- `Type`: Tipo de serviço;
- `WorkingDirectory`: Diretório do serviço;
- `ExecStart`: Comando para iniciar o serviço;
- `Restart`: Quando reiniciar o serviço;
- `RestartSec`: Tempo de espera em segundos entre as tentativas de reiniciar o serviço;
- `WantedBy`: Determina em quais condições o serviço será iniciado caso habilitado para iníciar automaticamente.

#### Exemplo de arquivo preenchido:

```
[Unit]
Description=NameBot
After=multi-user.target

[Service]
Type=simple
WorkingDirectory=/usr/local/bin/NameBot
ExecStart=/usr/bin/python3 /usr/local/bin/NameBot/namebot.py
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

Serviço de nome [NameBot](#1-o-bot-que-fala-com-as-pessoas), localizado na pasta */usr/local/bin/NameBot/*, e que deve ser reiniciado em 10 segundos caso pare por algum motivo.

### Carregar alterações

Reinicie o serviço do *systemd* **sempre** que um arquivo `.service` for alterado.

```shell
# systemctl daemon-reload
```

### Habilitar início automático

É necessário habilitar um serviço para que ele seja iniciado junto do sistema operacional. Ou seja, para que o serviço seja iniciado caso o servidor seja reiniciado.

```shell
# systemctl enable <nome do serviço>.service
```

### Iniciar serviço

```shell
# systemctl start <nome do serviço>.service
```

### Parar um serviço

```shell
# systemctl stop <nome do serviço>.service
```

### Reiniciar um serviço
```shell
# systemctl restart <nome do serviço>.service
```

### Verificar o status
```shell
# systemctl status <nome do serviço>.service
```

### Verificar logs
```shell
# journalctl -u <nome do serviço>.service
```
