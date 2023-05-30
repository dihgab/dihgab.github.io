---
layout: post
title: "Automação residencial: Introdução ao Home Assistant"
date: 2021-06-17 14:30:00 -0300
categories: [Automação Residencial, Home Assistant]
tags: automação homeassistant hass
image: "/assets/img/hass.png"
---

> Integrando tudo.

Home Assistant é um software de código aberto, grátis e que pode ser executado em um Raspberry Pi, em um computador ou em um conteiner Docker. Sua principal função é integrar tudo. Ele permite que diferentes dispositivos, de diferentes marcas, conversem e reajam juntos, conforme regras definidas pelo usuário.

Pode ser acessado via web ou por aplicativo. É uma solução bem completa de automação, identificando desde o status dos dispositivos até se a pessoa está andando ou dirigindo (rastreando o celular). Permite ser totalmente personalizado, ajustando-se cada item da interface de controle.

## Dispositivos de hospedagem

### Raspberry Pi

O Raspberry Pi é um computador do tamanho de um cartão de crédito e baixíssimo consumo energético. Pode ficar ligado 24 horas por dia e não impactar praticamente nada na conta de luz. A partir da versão 3 já é capaz de rodar uma instância do Home Assistant sem maiores problemas.

Caso opte por usar um Raspberry Pi, recomendo que use um dedicado ao Home Assistant. Basta baixar a imagem do sistema operacional já pronta no site, salvar em um cartão SD e ligar. Tudo estará preparado para ser usado.

### Computador

O Home Assistant pode funcionar em um computador, como se fosse um programa comum. A grande vantagem deste método é que computador, em geral, têm desempenho mais que suficiente para rodar uma instância de Home Assistant. A desvantagem é o volume do equipamento e, dependendo do caso, o consumo energético. No caso de computadores, geralmente não há problema em dar outros usos para máquina, não precisando mantê-la exclusivamente para o Home Assistant.

### Conteiner Docker

A maneira mais avançada. A única desvantagem é que exige alguns conhecimentos técnicos mais específicos. Porém, permite melhor uso dos recursos computacionais, facilitando o uso compartilhado da máquina.

## Instalação

Para garantir sempre uma instalação usando-se os recursos mais atualizados, recomendo sempre seguir o site oficial. 

[https://www.home-assistant.io/installation/](https://www.home-assistant.io/installation/)

Selecione o tipo de instalação e siga os passos descritos no site.

## Glossário

Termos importantes:

`Hass`: Abreviação de Home Assistant. É o *software* em si.

`Home Assistant Supervisor`: Parte responsável pelo controle do Hass. Sua instalação não é obrigatória, mas facilita muito para iniciantes.

`Hass.io`: Home Assistant somando ao *Supervisor*. É a solução pronta, que já contém os *addons* a um clique de distância.

`Addons`: Programas auxiliares ao Hass. Podem ser instalados também separadamente, sem usar a opção de addons. São exemplos: NodeRed e Mosquitto (MQTT).

`HassOS`: Sistema operacional. É baixada uma imagem já completa, com o Hass e o Hass.io prontos.

`Home Assistant Core`: Home Assistant que usa ambiente virtual *Python*.

`MQTT`: Protocolo de mensagens muito útil para troca de informações entre dispositivos. A comunicação acontece entre um cliente e um *broker*.

`Broker MQTT`: Pode ser entendido como um mural de avisos. É a parte responsável por receber as mensagens e deixá-las disponíveis para os demais dispositivos. Geralmente usa-se um software chamado [*Mosquitto*](https://mosquitto.org/).

`Cliente MQTT`: Dispositivo que lê e escreve no *broker*. São exemplos de clientes lâmpadas, fechaduras, sensores etc.

`NodeRed`: Software de desenvolvimento baseada em fluxos. Facilita a criação de automações e pode facilmente ser integrado ao Hass.

## Imagens

Exemplos de uso do Home Assistant.

### Controle de luzes

É possível controlar as luzes com clique na imagem, clique no botão ou até com automações.

<center>
<img src="/assets/img/hass1.gif" alt="Controle de luzes no Home Assistant." style="width:50%"> 
<br><small>Controle de luzes no Home Assistant.</small>
</center>

### Controle de televisão

A integração com *SmartTVs* é facilmente feita, não exigindo nenhum *hardware* adicional. Importante notar que uma vez desligado o aparelho, o comando de ligar irá funcionar somente se a televisão estiver ligada à rede usando-se um cabo. Pouquíssimas marcas aceitam o comando de ligar por wi-fi/sem fio.

<center>
<img src="/assets/img/hass2.gif" alt="Controle de televisão no Home Assistant" style="width:50%"> 
<br><small>Controle de televisão Home Assistant.</small>
</center>

### Informações gerais e contagem de tempo de uso

Outro uso interessante do Home Assistant é a contagem do tempo de uso dos equipamentos, permitindo até estimar o custo na conta de energia elétrica. Usando-se ferramentas externas é possível estimar tempo no trânsito ou preço de moedas e ações.

<center>
<img src="/assets/img/hass3.jpg" alt="Informações gerais no Home Assistant" style="width:50%"> 
<br><small>Informações gerais Home Assistant.</small>
</center>
