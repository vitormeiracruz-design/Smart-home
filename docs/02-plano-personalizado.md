# Plano personalizado (versão 1)

**Perfil:** casa própria, **sem reforma prevista**, 4 adultos, 1 gato, 1 cachorro, iPhone + Alexa, prefere **DIY de software** (configurar/programar) e **comprar pronto** tudo que envolva relé/eletrônica de potência. Prioridades: conforto + economia + segurança. Orçamento ainda indefinido.

> Valores em R$ são estimativas de ordem de grandeza. Confirmar preços e disponibilidade no Brasil antes de comprar.

## 1. Decisões de arquitetura

| Tema | Decisão | Por quê |
|---|---|---|
| Cérebro | **Home Assistant** em Raspberry Pi 4/5 (ou mini PC usado) com SSD | Local, sem mensalidade, integra tudo |
| iPhone | Expor o HA ao **Apple Home (HomeKit Bridge / Matter)** | Controle e automações pelo app Casa, Siri, widgets, Controle de Casa |
| Alexa | Expor ao Alexa via **Matter** (ou Nabu Casa, ~US$ 6,50/mês, opcional) | Aproveita a Alexa que você já tem, sem trocar nada |
| Dispositivos | **Zigbee como padrão** (coordenador USB), Wi-Fi só quando não houver alternativa | Barato, local, forma malha |
| "Sem reforma" | Priorizar produtos que **não exigem neutro** e módulos que cabem **atrás do interruptor existente** | Evita quebrar parede/passar fio |
| DIY | Só software (HA, automações, ESPHome). **Nada de montar relé/placa**: comprar módulos e sensores prontos | É o que você pediu |
| ESP32 | Fica opcional: usar **ESP32 prontos/prontos para ESPHome** só para casos sem produto pronto bom (ex.: ponte IR, sensor de nível) | Evitar solda |

### Ponto crítico: neutro nas caixas de luz
Em muitas casas brasileiras a caixa do interruptor tem só fase e retorno (sem neutro). Isso define o produto:
- **Sem neutro:** módulo mini Zigbee/Matter atrás do interruptor (tipo Sonoff ZBMINI-L, Aqara T1/T2, Shelly com ressalvas) ou **lâmpadas inteligentes** (interruptor físico fica sempre ligado).
- **Com neutro:** interruptor smart de parede (Zigbee) ou mini módulo.
- **Ação sua:** abrir **uma** caixa de interruptor por cômodo (com o disjuntor desligado, ou chamar eletricista) e ver se há fio azul/neutro. Isso muda a lista de compra.

## 2. Casa-mãe, cômodo por cômodo

### Sala + jantar (compartilhado)
- Luzes: 2–3 pontos com mini módulos atrás do interruptor ou lâmpadas Zigbee dimerizáveis.
- Cenas: "TV", "jantar", "visita", "boa noite" (1 botão sem fio na mesa/parede).
- TV/som: HDMI-CEC / integração Apple TV/Alexa; IR se necessário.
- Sensor de temperatura/umidade.
- Ar-condicionado (se houver): controle via ponte IR + automação.
- Fita LED atrás da TV/estante (opcional).

### Cozinha
- Luz com sensor de presença à noite (luz fraca).
- Sensor de vazamento sob a pia.
- Tomada medidora na geladeira/airfryer/micro-ondas (consumo e alertas).
- Sensor de fumaça/gás (alarme que também avisa o celular).
- Rotina: lembrete de "forno ligado" por consumo.

### Suíte (quarto + closet + banheiro)
- Luz com cenas (leitura, dormir, acordar gradual).
- Closet: luz por sensor de porta/presença.
- Banheiro: exaustor por umidade, luz noturna por presença.
- Ar-condicionado por IR + automação (desliga se janela aberta, liga antes de dormir).
- Botão de "boa noite" no criado-mudo (apaga tudo, tranca alertas, arma segurança).

### Quartos 1 e 2
- Mesmo padrão do quarto (luz + sensor de temperatura + ar-condicionado via IR).
- Opcional: cortina/persiana motorizada (maior custo; avaliar depois).

### Banheiro social
- Exaustor por umidade, luz noturna, sensor de porta (privacidade/ocupação).

### Lavanderia/área de serviço (se houver)
- Sensor de vazamento e tomada medidora na máquina ("ciclo terminou" por consumo).

## 3. Edícula (2 andares)

**Primeiro problema: rede.** A edícula fica separada da casa. Sem conectividade confiável ela vira "ilha".
Opções, da melhor para a pior:
1. **Cabo de rede (conduíte) enterrado ou aéreo** da casa até a edícula + AP Wi-Fi lá. Mais estável. Depende de passar o cabo (sem reforma de parede, mas é uma obra pequena externa).
2. **Mesh Wi-Fi** (Deco/Eero) com nó intermediário. Fácil, sem obra.
3. **Powerline** como último recurso.

**Zigbee:** coloque 2–3 dispositivos *com energia da tomada* (tomada smart/lâmpada) como roteadores para estender a malha até lá. Se mesmo assim falhar, um **segundo coordenador/ponte Zigbee-sobre-IP** (SLZB-06 ou similar) na edícula, ligado por cabo/Wi-Fi.

### Térreo (churrasqueira)
- Luzes com sensor de presença/crepuscular.
- Tomada smart para ventilador/exaustor.
- Sensor de temperatura/umidade.
- Câmera externa (se for cobrir quintal).
- Cuidado: calor e fumaça/gordura: usar caixas e produtos adequados; **não** instalar sensor comum de fumaça perto da churrasqueira (falso alarme).

### Sótão
- Sensor de temperatura/umidade (calor e mofo).
- Luz por presença.
- Sensor de vazamento/telhado (opcional).
- Ventilação/exaustor com automação por temperatura.

## 4. Pets (cachorro + gato)

- **Presença:** sensores PIR **imunes a pets** (algumas linhas têm modo pet) para segurança. Alternativa: sensores de porta/janela (não dão falso alarme com bicho).
- **Alimentação:** alimentador automático com Wi-Fi/Zigbee (existem opções com câmera e integração).
- **Água:** fonte inteligente ou sensor de nível.
- **Câmera interna** para checar os dois (modo "só quando todos saíram").
- **Pet door** (porta de gato inteligente) se houver espaço para instalação.
- **Cuidado:** modo "casa vazia" não deve desligar clima/água dos bichos; cuidar de calor no verão (sensor + ar-condicionado + alerta se a temperatura passar do limite).

## 5. Segurança (4 adultos, rotina variável)

- Sensores de porta/janela: entrada, fundos, edícula.
- Câmeras locais (RTSP) com detecção de pessoa (Frigate) quando houver orçamento.
- Alertas no iPhone (notificações críticas) + Alexa falando em alertas importantes.
- Fechadura inteligente: opcional; avaliar depois.
- Modos: **Casa** / **Dormindo** / **Fora** / **Viagem** (simulação de presença).
- Presença por celular (HA Companion no iPhone) por pessoa, para o modo "Fora" só ativar quando os 4 saírem.

## 6. Energia e economia

- Tomadas medidoras nos grandes consumidores: geladeira, freezer, ar-condicionado (se plugue), chuveiro **(ver abaixo)**, máquina de lavar, bomba d'água.
- Dashboard de consumo no HA com custo estimado em R$/mês.
- **Chuveiro elétrico:** carga alta (5–8 kW). **Não** usar tomada smart comum. Se quiser controlar, usar contator/relé de potência instalado por eletricista; pode ficar para uma fase futura.
- Medidor geral no quadro (clamp): ótimo para ver consumo total, mas instalar no quadro = **eletricista**.
- Se houver solar: integrar com o inversor (muitos têm API/Modbus).

## 7. Lista de compras sugerida por fases (estimativa)

### Fase 1 – Fundação (≈ R$ 900–1.800)
| Item | Qtd | Estimativa |
|---|---|---|
| Raspberry Pi 4/5 (4GB+) + fonte + caixa | 1 | R$ 500–900 |
| SSD USB/M.2 + adaptador (melhor que cartão SD) | 1 | R$ 150–300 |
| Coordenador Zigbee USB (SONOFF ZBDongle-E/P ou similar) | 1 | R$ 120–220 |
| Cabo de rede para o hub | 1 | R$ 20 |
| No-break pequeno para o hub e roteador | 1 | R$ 250–450 |

### Fase 2 – Ganhos rápidos (≈ R$ 700–1.800)
| Item | Qtd | Estimativa |
|---|---|---|
| Sensores de temperatura/umidade Zigbee | 6–8 | R$ 30–60 un. |
| Tomadas medidoras Zigbee | 4–6 | R$ 50–100 un. |
| Botões sem fio (cenas) | 3–4 | R$ 40–70 un. |
| Sensor de vazamento | 3 | R$ 60–100 un. |

### Fase 3 – Iluminação (≈ R$ 900–3.500)
| Item | Qtd | Estimativa |
|---|---|---|
| Mini módulos Zigbee/Matter para atrás do interruptor | 8–15 | R$ 60–150 un. |
| ou lâmpadas Zigbee dimerizáveis | alguns | R$ 40–120 un. |
| Sensores de presença (corredor/banheiro/closet/sótão) | 4–6 | R$ 50–100 un. |

### Fase 4 – Clima (≈ R$ 400–1.200)
| Item | Qtd | Estimativa |
|---|---|---|
| Ponte IR (Broadlink/ou ESP32 pronto com IR) para cada ar-condicionado/TV | 1 por cômodo | R$ 80–180 un. |
| Exaustores com módulo | 2–3 | R$ 100–300 |

### Fase 5 – Segurança e pets (≈ R$ 1.500–5.000)
| Item | Qtd | Estimativa |
|---|---|---|
| Sensores de porta/janela | 6–10 | R$ 40–70 un. |
| Sensor de fumaça/gás conectado | 2 | R$ 100–250 un. |
| Câmeras (interna + externa) | 2–4 | R$ 200–600 un. |
| Alimentador automático | 1–2 | R$ 300–900 |
| Fechadura inteligente | 0–1 | R$ 600–1.500 |

### Fase 6 – Edícula e rede (≈ R$ 500–2.500)
| Item | Qtd | Estimativa |
|---|---|---|
| Mesh Wi-Fi com ponto na edícula **ou** AP + cabo | 1 conjunto | R$ 600–1.800 |
| Roteadores Zigbee (tomadas/lâmpadas) para estender malha | 2–3 | R$ 50–100 un. |
| Eventual ponte Zigbee-sobre-IP | 0–1 | R$ 250–450 |

**Total geral estimado:** da ordem de **R$ 5.000 a R$ 15.000** conforme escolhas (câmeras, fechadura, quantidade de pontos de luz). Dá para começar com **Fases 1+2 (~R$ 1.600–3.600)** e já ter sensores, medição de energia e cenas funcionando.

## 8. Ordem de implantação recomendada

1. **Fundação:** HA, Zigbee, backup, no-break.
2. **Sensores e medição** (baixo risco, alto valor): temperatura, vazamento, tomadas medidoras.
3. **Luz + cenas** nos cômodos de maior uso (sala, suíte, cozinha).
4. **Clima por IR** nos cômodos com ar-condicionado.
5. **Edícula** (rede + malha Zigbee) e depois churrasqueira/sótão.
6. **Segurança + pets**.
7. **Integração Apple Home + Alexa** (pode começar já na fase 1).
8. **Extras** (fechadura, cortina, solar, painel de parede).

## 9. Riscos e cuidados específicos

- **iPhone + Alexa juntos:** ficar com *um* sistema de verdade (HA) e deixar Apple Home/Alexa apenas como "interfaces". Evita automações duplicadas.
- **Sem neutro:** erro mais comum é comprar interruptor que exige neutro. Verificar antes.
- **Interruptor físico:** manter o interruptor funcionando (módulo "atrás" dele) para os outros 3 adultos não precisarem de app.
- **Aceitação da família:** priorizar automações **invisíveis** (luz por presença) e botões físicos; evitar exigir app para tarefas básicas.
- **Pets:** sensores de movimento sofrem falso alarme; usar sensores de abertura para segurança e movimento só para conforto.
- **Quadro elétrico, chuveiro, bombas:** eletricista.
- **Wi-Fi na edícula:** testar o sinal com o celular antes de comprar qualquer dispositivo Wi-Fi para lá.

## 10. Perguntas que faltam

1. **Orçamento** aproximado para a fase inicial (mesmo que seja "até R$ 3 mil, depois vamos vendo")?
2. **Ar-condicionado:** quantos e em quais cômodos? Splits (controle remoto IR) ou janela?
3. **Solar ou chuveiro elétrico?** Aquecimento é elétrico, a gás ou solar térmico?
4. **Neutro:** você consegue verificar uma caixa de interruptor (ou prefere eu priorizar lâmpadas e módulos sem neutro)?
5. **Wi-Fi atual:** qual roteador/modelo? Cobre a edícula hoje? O sinal chega lá?
6. **Garagem/portão/interfone:** existe portão eletrônico? Interfone? Quer integrar?
7. **Prioridade imediata:** o que mais te incomoda hoje (conta de luz, esquecer luz acesa, segurança, pets, conforto)?
8. **Alexa:** qual modelo (Echo Dot, Show, etc.)? Quantas unidades?
9. **Câmeras:** aceita câmeras dentro de casa (privacidade dos 4 adultos)? Alguma já instalada?
