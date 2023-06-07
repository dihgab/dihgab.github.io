---
layout: post
title: "Automação residencial: Como fazer uma planta baixa no HomeAssistant"
date: 2021-07-23 19:30:00 -0300
categories: [Automação Residencial, Home Assistant]
tags: automação hass homeassistant plantabaixa
image: "/assets/img/hass1_bg.png"
---

## Interface

Um dos pontos que mais chamam a atenção na interface do Home Assistant é a possibilidade de se ter uma planta baixa do imóvel, que exibe, em tempo real, o estado dos dispositivos inteligentes. É uma maneira simples e bonita de se controlar tudo.

## Cartão

O cartão correto a ser utilizado é o `picture-elements`, cuja documentação está disponível no [site oficial](https://www.home-assistant.io/lovelace/picture-elements/).

```yaml
image: /local/planta/planta_completa.png
type: picture-elements
title: Luzes
```

* `type`: Tipo de cartão.
* `image`: A imagem que será colocada no fundo.
  * A imagem deve ser colocada em uma pasta chamada `www` na raíz do Home Assistant, mesmo local em que fica o `configuration.yml`.
* `title`: Título do cartão.

### Elementos

Cada objeto colocado na imagem é um elemento, que contém uma entidade já configurada em seu Home Assistant. O primeiro exemplo é uma lâmpada com os estados mostrados em diferentes imagens. Já o segundo é uma lâmpada ilustrada com um ícone, que muda de cor de acordo com o funcionamento. Por fim, o terceiro é um ícone que, ao ser clicado, liga uma televisão.

```yaml
elements:
  - entity: switch.sonoff1
    type: image
    state_image:
      'off': /local/off.png
      'on': /local/on.png
    style:
      left: 40%
      top: 50%
      width: 100%
  - entity: switch.sonoff2
    icon: mdi:car
    style:
      left: 80%
      top: 70%
    tap_action:
      action: toggle
    type: state-icon
  - entity: remote.sala
    icon: mdi:power-cycle
    style:
      '--iron-icon-height': 40px
      '--iron-icon-width': 40px
      color: red
      left: 10%
      top: 10%
    tap_action:
      action: call-service
      service: remote.send_command
      service_data:
        command: turn_on
        delay_secs: 0.75
        device: lg
        entity_id: remote.sala
        hold_secs: 1
        num_repeats: 1
```

* `entity`: A entidade a ser exibida
* `tap_action`: Ação ao tocar.
  * `toggle` faz inverter o estado atual (ligar/desligar).
  * `call-service`: Executa o serviço.
    * Consulte a configuração de acordo com o serviço para preencher os dados necessários.
* `type`: O que é exibido.
  * `image` para imagem
    * `off`: Imagem a ser exibida quando entidade estiver desligada.
    * `on`: Imagem exibida quando ligar a entidade.
  * `state-icon` para ícone.
    * O ícone pode ser definido em `icon` ou nas customizações.
* `style`: Personalização do ícone ou da imagem.
  * `left`: Distância do canto esquerdo medido em porcentagem (%).
  * `top`: Distância do topo em porcentagem (%).
  * `color`: Cor do ícone.

### Exemplo

Outro uso do `picture-elements` é a criação de controles diversos. Abaixo um exemplo de controle remoto simples para tv, com parte do *YAML* mostrando como foi feito. O serviço usado parte de um dispositivo Broadlink Mini IR, que funciona como um controle remoto infravermelho.

<center>
<img src="/assets/img/hass_tv_control.png" alt="Exemplo de controle remoto usando Picture Elements." style="width:50%"> 
<br><small>Exemplo de controle remoto usando Picture Elements.</small>
</center>

```yaml
image: /local/background.png
title: TV LG
type: picture-elements
elements:
  - entity: remote.sala
    type: icon
    icon: mdi:power-cycle
    style:
      '--iron-icon-height': 40px
      '--iron-icon-width': 40px
      color: red
      left: 10%
      top: 10%
    tap_action:
      action: call-service
      service: remote.send_command
      service_data:
        command: turn_on
        delay_secs: 0.75
        device: lg
        entity_id: remote.sala
        hold_secs: 1
        num_repeats: 1
  - entity: remote.sala
    type: icon
    icon: mdi:numeric-0
    style:
      '--iron-icon-height': 40px
      '--iron-icon-width': 40px
      color: black
      left: 80%
      top: 50%
    tap_action:
      action: call-service
      service: remote.send_command
      service_data:
        command: 0
        delay_secs: 0.75
        device: lg
        entity_id: remote.sala
        hold_secs: 1
        num_repeats: 1
```
