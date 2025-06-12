# 🌡️🔦 Monitoramento — Desafio 01 CBTIAAP

Protótipo **Arduino UNO** que monitora **luminosidade, temperatura (DHT11) e umidade**. O estado atual é exibido em um **LCD 16 × 2**; quando algum parâmetro sai da faixa segura, **LEDs** mudam de cor e um **buzzer** dispara.

> Este README segue as <a href="https://www.alura.com.br/artigos/escrever-bom-readme">boas práticas da Alura</a> 📚

---

## ✨ Funcionalidades principais

| # | Recurso                        | Descrição                                                   |
| - | ------------------------------ | ----------------------------------------------------------- |
| 1 | Média de 5 leituras ⏱️         | Suaviza ruídos a cada 5 s (`millis`).                       |
| 2 | Três faixas de luminosidade 💡 | Verde ≤ 30 %, Amarelo 31 – 69 %, Vermelho ≥ 70 %.           |
| 3 | Faixa ideal de temperatura 🌡️ | 10 – 15 °C. Fora disso: alerta.                             |
| 4 | Faixa ideal de umidade 💧      | 50 – 70 %. Fora disso: alerta.                              |
| 5 | LCD cíclico 📺                 | Alterna Luz → Temperatura → Umidade a cada ciclo.           |
| 6 | Hierarquia de alertas 🔔       | Temperatura > Umidade > Luminosidade para evitar conflitos. |

---

## 🔌 Componentes

| Qtde   | Componente                  | Função                         |
| ------ | --------------------------- | ------------------------------ |
| 1      | Arduino UNO                 | Processamento e I/O            |
| 1      | Sensor **DHT11**            | Temperatura (°C) + Umidade (%) |
| 1      | **Fotorresistor** + R 10 kΩ | Luminosidade (%)               |
| 3      | **LEDs** (G/Y/R)            | Indicadores visuais            |
| 1      | **Buzzer passivo**          | Alerta sonoro                  |
| 1      | **LCD 16×2** (modo 4 bits)  | Interface textual              |
| 1      | Potenciômetro 10 kΩ         | Ajuste de contraste do LCD     |
| vários | Resistores 220 Ω            | Limite de corrente para LEDs   |
| 1      | Breadboard + jumpers        | Montagem                       |

### Mapa de pinos Arduino ↔ componentes

| Pino Arduino | Componente    | Observação |
| ------------ | ------------- | ---------- |
| 12           | LCD RS        |            |
| 10           | LCD E         |            |
| 5–2          | LCD D4–D7     |            |
| 6            | Buzzer        |            |
| 7            | LED Verde     |            |
| 8            | LED Amarelo   |            |
| 9            | LED Vermelho  |            |
| A0           | DHT11 (DATA)  |            |
| A1           | Fotorresistor |            |
| 5 V / GND    | Todos         |            |

---

## ▶️ Como rodar

### 1. Simulação no Tinkercad

```text
1. Abra https://www.tinkercad.com/things/dUIiLKyi1jI](https://www.tinkercad.com/things/dUIiLKyi1jI-desafio-automacao-perifericos/editel?returnTo=%2Fthings%2FdUIiLKyi1jI&sharecode=d9JQxzLJblUT5D6rpTXKYEm6FmJUI7h6LQAqyx3F1qk)
2. Clique em **Start Simulation**
3. Use os sliders do sensor DHT11 e da luz para reproduzir os cenários abaixo.
4. Segue vídeo https://www.youtube.com/watch?v=fpgiWTr8pxE
```

## 🖼️ Cenários demonstrativos

| Cenário                            | Print                                                                                                 | O que observar                                         |
| ---------------------------------- | ------------------------------------------                                                            | ------------------------------------------------------ |
| **Visão geral**                    | ![image](https://github.com/user-attachments/assets/b3b340b3-1c8b-42ab-8201-abc3c9499d76)             | Arranjo físico dos componentes                         |
| **Luminosidade — Ambiente escuro** | ![image](https://github.com/user-attachments/assets/bb1367ef-8d98-45a6-95a7-87909a7f3f82)             | LCD exibe "Ambiente escuro" · LED verde aceso          |
| **Temperatura alta (> 15 °C)**     | ![image](https://github.com/user-attachments/assets/0f92cd50-006b-49ed-a6e2-7c9e1bbca8d3)             | LCD exibe "Temp. Alta" · LED amarelo · buzzer ativo    |
| **Umidade alta (> 70 %)**          | ![image](https://github.com/user-attachments/assets/9d45c726-5cf9-40e3-af36-a78a18c6bbe2)             | LCD exibe "Umidade Alta" · LED vermelho · buzzer ativo |


## 🚧 Desafios & soluções

| Desafio               | Abordagem                                    |
| --------------------- | -------------------------------------------- |
| Ruído nas leituras    | Média de 5 amostras + pequeno `delay(100)`   |
| Conflito de saídas    | Prioridade Temp > Umid > Luz                 |
| Estabilidade do DHT11 | Delay inicial de 2 s + verificação `isnan()` |
| Contraste do LCD      | Potenciômetro ligado entre V0 e GND/5 V      |

---------------------------------------------------------------------------------------

### Texto explicativo – Projeto de Automação com Arduino

Este projeto demonstra como **automatizar o monitoramento de um ambiente** utilizando uma placa **Arduino UNO** e alguns componentes eletrônicos de baixo custo. A ideia é criar um sistema autônomo que:

1. **Meça em tempo real** três variáveis essenciais — **luminosidade, temperatura e umidade**;
2. **Analise** se cada variável está dentro da faixa de operação configurada;
3. **Alerta visual e sonoramente** sempre que algum valor sai do limite seguro;
4. **Exiba informações** claras em um **display LCD 16×2**, para que qualquer pessoa possa verificar o estado do ambiente de relance.

---

#### Como o sistema funciona (fluxo de automação)

1. **Coleta de dados**

   * **Fotoresistor** lê o nível de luz ambiente (0 % – 100 %).
   * **Sensor DHT11** fornece temperatura em °C e umidade relativa em %.
   * Cada sensor é lido **cinco vezes** a cada cinco segundos; as leituras são somadas e a média é calculada para filtrar ruídos.

2. **Processamento & decisão**

   * O código compara os valores médios com faixas ideais:

     * **Luminosidade**:

       * 0 – 30 %  → ambiente escuro (LED verde).
       * 31 – 69 % → meia-luz (LED amarelo).
       * ≥ 70 %     → muito claro (LED vermelho + buzzer).
     * **Temperatura**:

       * 10 – 15 °C → ideal.
       * < 10 °C   → temperatura baixa (LED amarelo + buzzer).
       * > 15 °C   → temperatura alta (LED amarelo + buzzer).
     * **Umidade**:

       * 50 – 70 % → ideal.
       * < 50 %   → umidade baixa (LED vermelho + buzzer).
       * > 70 %   → umidade alta (LED vermelho + buzzer).

   * Para evitar que dois alertas diferentes acendam LEDs ao mesmo tempo, foi criada uma **hierarquia de prioridade**:
     **Temperatura** > **Umidade** > **Luminosidade**.

3. **Ação & feedback**

   * **LEDs** indicam rapidamente o estado geral (verde = OK, amarelo = atenção, vermelho = crítico).
   * **Buzzer** emite som contínuo quando há condição crítica.
   * **LCD** alterna automaticamente entre três telas:

     1. Situação de luminosidade;
     2. Situação da temperatura;
     3. Situação da umidade.
        Cada tela mostra texto descritivo (“Ambiente escuro”, “Temp. Alta”, “Umidade Alta”, etc.) e o valor numérico correspondente.

---

#### Componentes utilizados

| Componente                          | Função no sistema                                     |
| ----------------------------------- | ----------------------------------------------------- |
| **Arduino UNO**                     | Cérebro do projeto; lê sensores e controla atuadores. |
| **DHT11**                           | Mede temperatura e umidade.                           |
| **Fotoresistor + resistor 10 kΩ**   | Converte intensidade de luz em sinal analógico.       |
| **LCD 16×2**                        | Mostra mensagens e valores para o usuário.            |
| **LEDs verde / amarelo / vermelho** | Feedback visual rápido.                               |
| **Buzzer passivo**                  | Alerta sonoro quando algo está fora dos limites.      |
| **Potenciômetro 10 kΩ**             | Ajusta contraste do LCD.                              |
| **Resistores 220 Ω**                | Limitam corrente dos LEDs.                            |
| **Breadboard + jumpers**            | Facilita a montagem sem solda.                        |

---

#### Demonstrações (prints)

1. **Luminosidade – Ambiente escuro**
   *LCD exibe:* “Luminosidade: Ambiente escuro”
   *LED ativo:* Verde

2. **Temperatura – Alta**
   *LCD exibe:* “Temp. Alta – 24 °C”
   *LED ativo:* Amarelo *Buzzer ligado*

3. **Umidade – Alta**
   *LCD exibe:* “Umidade Alta – 90 %”
   *LED ativo:* Vermelho *Buzzer ligado*

> Essas capturas ajudam a comprovar que o sistema reage corretamente a cada condição testada.

---

**Resumo:** este projeto prova que, com poucos componentes e a flexibilidade do Arduino, é possível **automatizar o acompanhamento de variáveis ambientais**, alertar usuários em tempo real e exibir dados de forma intuitiva, servindo de base para sistemas maiores — desde estufas e terrários até pequenos laboratórios ou salas de TI.

**Integrantes:**
Alice da Silva Marinho Gomes - CB3025772
Matheus Leandro Terra Luciano - CB3024881
Milena Costa de Andrade - CB3024881
Vinicius do Nascimento Ayres - CB3025675
