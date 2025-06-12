# 🔧 Monitoramento Ambiental – Desafio 01 CBTIAAP

Protótipo em Arduino que **lê luminosidade, temperatura (DHT11) e umidade** e informa, em tempo real, se cada variável está dentro dos limites configurados. Tudo é mostrado em um **LCD 16×2**; quando algo sai do aceitável, **LEDs** mudam de cor e um **buzzer** dispara.

> Este README segue boas práticas descritas no artigo da Alura sobre documentação de projetos.

---

## 📌 Funcionalidades

* Média de 5 leituras a cada 5 s para suavizar ruídos.
* Três faixas de luminosidade → LEDs verde/ amarelo/ vermelho.
* Faixas ideias de temperatura (10 – 15 °C) e umidade (50 – 70 %) exibidas no LCD.
* Alertas visuais/sonoros automáticos fora dos limites.
* LCD alterna automaticamente entre **Luz → Temperatura → Umidade**.
* Hierarquia de prioridade de alertas para evitar conflito de LEDs/buzzer.

---

## 🧩 Componentes

|     Qtde | Componente                         | Função                          |
| -------: | ---------------------------------- | ------------------------------- |
|        1 | Arduino UNO                        | Processamento e I/O.            |
|        1 | Sensor **DHT11**                   | Temperatura (°C) & Umidade (%). |
|        1 | **Fotorresistor** + resistor 10 kΩ | Mede intensidade luminosa.      |
|        3 | LEDs (verde, amarelo, vermelho)    | Indicadores de status.          |
|        1 | **Buzzer passivo**                 | Alarme sonoro.                  |
|        1 | **LCD 16×2** (modo 4 bits)         | Feedback textual ao usuário.    |
|        1 | Potenciômetro 10 kΩ                | Ajuste de contraste do LCD.     |
| Diversos | Resistores 220 Ω                   | Limitam corrente nos LEDs.      |
|        1 | Breadboard + jumpers               | Montagem sem solda.             |

### Mapa de pinos

| Arduino   | Conectado a          | Descrição |
| --------- | -------------------- | --------- |
| 12        | LCD RS               |           |
| 10        | LCD E                |           |
| 5 – 2     | LCD D4 – D7          |           |
| 6         | Buzzer               |           |
| 7         | LED Verde            |           |
| 8         | LED Amarelo          |           |
| 9         | LED Vermelho         |           |
| A0        | DHT11 data           |           |
| A1        | Fotorresistor        |           |
| GND / 5 V | Todos os componentes |           |

> **Dica:** conecte os pinos extremos do potenciômetro a 5 V e GND e o pino central ao V0 do LCD para regular contraste.

![Circuito montado](docs/circuit.png)

---

## 🚀 Como executar

### 1. Simulação (Tinkercad)

1. Acesse o link: [https://www.tinkercad.com/things/dUIiLKyi1jI](https://www.tinkercad.com/things/dUIiLKyi1jI).
2. Clique em **Start Simulation**.
3. Alterne os sliders do **DHT11** (temp/umidade) e da **luz** para ver as reações no LCD, LEDs e buzzer.

### 2. Placa real

1. Baixe `monitoramento_ambiental.ino`.
2. Grave no Arduino UNO.
3. Monte o circuito com a pinagem acima.
4. Alimente com 5 V (USB ou fonte externa).

---

## 🛠️ Dificuldades encontradas e soluções

| Desafio                                         | Solução                                                                                          |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| **Ruído nas leituras** do DHT11 e fotorresistor | Média de 5 amostras e `delay(100)` entre leituras.                                               |
| **Conflito de saídas** (múltiplos alertas)      | Prioridade Temperatura > Umidade > Luz e atualização dos LEDs só após avaliar todos os sensores. |
| **Estabilidade do DHT11** no Tinkercad          | Aguardado 2 s no `setup()` + verificação de `isnan()` nas leituras.                              |
| **Contraste do LCD**                            | Potenciômetro de 10 kΩ em V0.                                                                    |


