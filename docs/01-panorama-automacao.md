# Panorama: o que dá para automatizar na casa

> Primeira missão: mostrar o leque de possibilidades e responder se um projeto grande precisa de mais de uma placa.
> Valores em R$ são **estimativas de ordem de grandeza** (variam por loja/época). Confirmar preços antes de comprar.

## 1. Resposta curta: uma placa só ou várias?

**Várias, mas com papéis diferentes.** Uma casa bem automatizada raramente usa uma placa só. O desenho que mais funciona:

| Camada | O que faz | Hardware típico |
|---|---|---|
| **Cérebro (hub)** | Roda as automações, painel, histórico, acesso remoto | Raspberry Pi 4/5 ou mini PC usado (Home Assistant) |
| **Nós/sensores** | Falam com o mundo físico (relés, sensores, LEDs) | ESP32 / ESP8266 (um por cômodo ou por função) |
| **Dispositivos prontos** | Lâmpadas, tomadas, sensores comerciais | Zigbee / Wi-Fi / Matter |
| **Rede** | Liga tudo | Roteador Wi-Fi decente + (opcional) cabo nos pontos críticos |

Ou seja: **1 hub + N ESP32 + dispositivos prontos** onde não vale a pena fazer na mão.

## 2. Qual placa para quê

| Placa | Ponto forte | Use para | Evite para |
|---|---|---|---|
| **ESP32** (~R$ 30–50) | Wi-Fi + Bluetooth, barato, ótimo com ESPHome | Sensores, relés, controle de luz/cortina, IR, ponte BLE | Rodar o sistema central |
| **ESP8266** (~R$ 20–30) | Mais simples/barato | Tomada inteligente DIY, relé simples | Projetos que precisam de BLE ou muitos pinos |
| **Arduino (Uno/Nano)** (~R$ 40–80) | Robusto, muitos pinos, tempo real | Controle de motores, irrigação em campo, painéis com muitos I/O | Wi-Fi (precisa de módulo extra) |
| **Raspberry Pi 4/5** (~R$ 500–900) | Linux completo | Hub Home Assistant, Frigate, servidor | Ler sensor analógico direto (sem ADC) |
| **Raspberry Pi Zero 2 W** | Pequeno, Linux | Câmera, AirPlay/áudio, mini-servidor | Hub principal com muitos dispositivos |
| **Raspberry Pi Pico W** | Microcontrolador barato com Wi-Fi | Alternativa ao ESP32 | Quando precisa do ecossistema ESPHome (menos maduro) |
| **Mini PC usado (N100 / i3)** (~R$ 600–1.200) | Muito mais potência que o Pi, SSD | Hub robusto + câmeras com IA | Quem quer consumo mínimo |

**Recomendação de partida:** Home Assistant em um Pi 4/5 (ou mini PC) + ESP32 com **ESPHome** + alguns dispositivos Zigbee. Arduino só entra se houver um caso específico de muitos pinos/tempo real.

## 3. Protocolos (a escolha mais importante)

| Protocolo | Prós | Contras | Quando usar |
|---|---|---|---|
| **Wi-Fi** | Sem hub extra, fácil | Congestiona a rede, depende da nuvem em produtos comerciais | ESP32 DIY, câmeras |
| **Zigbee** | Barato, baixo consumo, rede em malha, 100% local | Precisa de coordenador USB (~R$ 120–200) | Sensores, lâmpadas, interruptores, tomadas |
| **Matter / Thread** | Padrão novo, interoperável | Ainda amadurecendo, poucos produtos baratos | Futuro; comprar com cuidado |
| **Bluetooth/BLE** | Sensores de temperatura baratos | Alcance curto | Termômetros, presença por proximidade |
| **MQTT** | Mensageria simples entre dispositivos DIY | Requer broker (Mosquitto, roda no hub) | Integrar nós DIY (ou usar API nativa do ESPHome) |
| **Cabeado (RS485/KNX/PoE)** | Confiabilidade máxima | Só vale em obra/reforma | Casa em construção |
| **IR / RF 433 MHz** | Controla o que é "burro" (ar-condicionado, portão, TV) | Sem retorno de estado | Ar-condicionado, ventilador, portão |

Princípio: **priorize controle local** (funcionar sem internet) e evite ficar refém de nuvem de fabricante.

## 4. Catálogo do que dá para fazer

Legenda de complexidade: 🟢 fácil · 🟡 médio · 🔴 avançado. Custo = por ponto/unidade, estimado.

### 💡 Iluminação
- Lâmpadas inteligentes Zigbee/Wi-Fi, fitas LED endereçáveis (ESP32 + WLED) 🟢 — R$ 30–120/ponto
- Interruptor inteligente (módulo relé atrás da tomada/interruptor existente) 🟡 — R$ 60–150
- Luz por presença/horário/luminosidade 🟢 (só configuração)
- Cenas ("cinema", "boa noite", "acordar") 🟢

### 🔌 Energia e tomadas
- Tomadas inteligentes com medição de consumo 🟢 — R$ 50–100
- Medidor de energia do quadro (clamps SCT-013 + ESP32 ou medidor Zigbee) 🟡 — R$ 150–400 (**quadro elétrico: só com eletricista**)
- Desligar standby, agendar cargas, dashboard de consumo/custo 🟢
- Integração com energia solar / inversor (se houver) 🟡

### 🌡️ Clima e conforto
- Sensores de temperatura/umidade (Zigbee/BLE) 🟢 — R$ 30–60
- Controle de ar-condicionado via IR (ESP32 + emissor, ou Broadlink) 🟡 — R$ 60–150
- Ventilador/exaustor por temperatura ou umidade 🟡
- Qualidade do ar (CO₂, PM2.5) 🟡 — R$ 150–400

### 🪟 Cortinas, persianas, portões
- Motor de cortina/persiana Zigbee ou kit DIY 🟡 — R$ 250–800
- Portão de garagem (módulo relé em paralelo com controle) 🟡 — R$ 60–150
- Fechadura inteligente 🟡 — R$ 400–1.500

### 🔒 Segurança
- Sensores de porta/janela 🟢 — R$ 40–70
- Sensores de movimento (PIR) 🟢 — R$ 40–70
- Câmeras locais (RTSP/PoE) + Frigate (detecção de pessoa) 🔴 — R$ 200–600/câmera
- Sirene, alertas no celular, simulação de presença ao viajar 🟢
- Sensores de fumaça/gás/vazamento de água 🟢 — R$ 80–200

### 🌱 Jardim e água
- Irrigação automática (válvulas solenoides + sensor de umidade do solo) 🟡 — R$ 200–600
- Nível de caixa d'água/cisterna (sensor ultrassônico) + bomba 🟡 — R$ 100–300
- Sensor de vazamento sob pia/máquina 🟢

### 🔊 Mídia e rotina
- Controle de TV/som (IR, HDMI-CEC, integrações) 🟡
- Assistente de voz **local** (Home Assistant Voice) ou Alexa/Google 🟡
- Notificações faladas ("a máquina de lavar terminou") 🟢

### 🐾 Extras úteis
- Alimentador de pets, monitor de caixa de areia 🟡
- Presença por celular/Wi-Fi/BLE (casa vazia → modo economia) 🟢
- Campainha com câmera 🟡
- Painel em tablet na parede 🟡 — R$ 300–700

## 5. Arquitetura sugerida (modelo)

```
                 Internet (opcional: acesso remoto seguro / VPN)
                                  │
                         ┌────────┴────────┐
                         │  Roteador Wi-Fi │
                         └────────┬────────┘
              ┌───────────────────┼────────────────────┐
     ┌────────┴────────┐  ┌───────┴───────┐   ┌────────┴────────┐
     │ HUB: Home       │  │ Coordenador   │   │ Câmeras (PoE/   │
     │ Assistant       │──│ Zigbee (USB)  │   │ RTSP)           │
     │ (Pi / mini PC)  │  └───────┬───────┘   └─────────────────┘
     └────────┬────────┘          │ malha Zigbee
              │ Wi-Fi             ├─ sensores, lâmpadas, tomadas
     ┌────────┴─────────┐         └─ interruptores, botões
     │ ESP32 (ESPHome)  │
     │ relés, IR, LEDs, │
     │ sensores DIY     │
     └──────────────────┘
```

## 6. Faixas de investimento (estimativas)

| Nível | O que inclui | Faixa aproximada |
|---|---|---|
| **Starter** | Hub + coordenador Zigbee + ~10 dispositivos (luz, tomadas, sensores) | R$ 1.000 – 2.500 |
| **Intermediário** | + clima (IR), cortina, medição de energia, alguns sensores de segurança | R$ 3.000 – 7.000 |
| **Casa completa** | + câmeras, irrigação, fechadura, painel, voz local, maior cobertura | R$ 8.000 – 20.000+ |

Em obra/reforma, prever **eletroduto com folga, neutro nas caixas de interruptor e pontos de rede** custa muito pouco agora e economiza muito depois.

## 7. Roadmap em fases (sugestão)

1. **Fundação:** hub (Home Assistant), rede estável, coordenador Zigbee, backup.
2. **Ganhos rápidos:** tomadas medidoras, sensores de temperatura, luzes de 1–2 cômodos, cenas.
3. **Conforto:** ar-condicionado via IR, cortinas, automações por presença/horário.
4. **Segurança:** sensores de porta/janela, fumaça/vazamento, câmeras locais.
5. **Avançado:** energia (quadro/solar), irrigação, voz local, painel de parede.

## 8. Cuidados importantes

- **Corrente alternada (127/220 V):** mexer em quadro/fiação só com eletricista habilitado. Para DIY, prefira módulos certificados em vez de relé solto.
- **Segurança digital:** troque senhas padrão, mantenha tudo atualizado, não exponha o hub direto à internet (use VPN/Tailscale ou Nabu Casa).
- **Falha segura:** luz e fechadura precisam continuar funcionando manualmente se o sistema cair.
- **Evitar dependência de nuvem:** dê preferência a Zigbee/local.
- **Backup** da configuração do Home Assistant desde o início.

## 9. O que preciso saber de você (próximo passo)

1. **Tipo de moradia:** casa ou apartamento? própria ou alugada? obra/reforma à vista?
2. **Tamanho:** número de cômodos, andares, área externa (quintal/garagem/jardim)?
3. **Prioridades:** conforto, economia de energia, segurança, ou "tecnologia pela tecnologia"?
4. **Orçamento:** inicial e mensal/gradual.
5. **Nível técnico e tempo:** quer soldar/programar (DIY) ou prefere produtos prontos?
6. **O que já tem:** roteador, assistente de voz (Alexa/Google), ar-condicionado, solar, TVs smart?
7. **Quem usa:** família, crianças, idosos, pets (influencia segurança e simplicidade)?
8. **Ecossistema do celular:** Android ou iPhone?
9. **Internet:** operadora estável? Há queda de energia frequente (no-break)?

Com essas respostas eu monto: lista de materiais com custo por cômodo, diagrama da rede e plano de implantação por fases.
