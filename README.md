🔧 Monitoramento Ambiental – Desafio 01 CBTIAAP

Protótipo em Arduino que lê luminosidade, temperatura (DHT11) e umidade e informa, em tempo real, se cada variável está dentro dos limites configurados. Tudo é mostrado em um LCD 16×2; quando algo sai do aceitável, LEDs mudam de cor e um buzzer dispara.

Este README segue boas práticas descritas no artigo da Alura sobre documentação de projetos.

📌 Funcionalidades

Média de 5 leituras a cada 5 s para suavizar ruídos.

Três faixas de luminosidade → LEDs verde/ amarelo/ vermelho.

Faixas ideias de temperatura (10 – 15 °C) e umidade (50 – 70 %) exibidas no LCD.

Alertas visuais/sonoros automáticos fora dos limites.

LCD alterna automaticamente entre Luz → Temperatura → Umidade.

Hierarquia de prioridade de alertas para evitar conflito de LEDs/buzzer.

🧩 Componentes e o que faz cada um

Qtde

Componente

Função no sistema

1

Arduino UNO

Cérebro do projeto – lê sensores, decide alertas e atualiza LCD.

1

Sensor DHT11

Mede temperatura (°C) e umidade relativa (%).

1

Fotorresistor + resistor 10 kΩ

Varia a resistência conforme a luz; convertemos a leitura analógica em % de luminosidade.

3

LEDs (verde, amarelo, vermelho)

Indicadores visuais rápidos: verde = ok/escuro, amarelo = meia‑luz, vermelho = alerta.

1

Buzzer passivo

Emite som quando qualquer variável sai da faixa segura.

1

LCD 16×2 (modo 4 bits)

Interface textual: mostra status de luz, temperatura ou umidade.

1

Potenciômetro 10 kΩ

Ajusta contraste do LCD.

Diversos

Resistores 220 Ω

Limitam corrente dos LEDs e protegem o fotorresistor.

1

Breadboard + jumpers

Montagem sem solda, facilitando alterações.

Mapa de pinos

Arduino

Conectado a

Descrição

12

LCD RS

10

LCD E

5 – 2

LCD D4 – D7

6

Buzzer

7

LED Verde

8

LED Amarelo

9

LED Vermelho

A0

DHT11 data

A1

Fotorresistor

GND / 5 V

Todos os componentes



🚀 Como executar

1. Simulação (Tinkercad)

Acesse: https://www.tinkercad.com/things/dUIiLKyi1jI.

Clique em Start Simulation.

Use os sliders do DHT11 e da luz para testar os três cenários descritos abaixo.

2. Placa real

Baixe monitoramento_ambiental.ino.

Grave no Arduino UNO.

Monte o circuito conforme o diagrama acima.

🖼️ Demonstrações (prints)

Salve as quatro imagens em docs/ e referencie conforme abaixo.

1. Visão geral do circuito
![image](https://github.com/user-attachments/assets/6e87a8c3-7c7e-4cf4-9b1f-b97411ae2ef2)

2. Cenário – Luminosidade (Ambiente escuro)
![image](https://github.com/user-attachments/assets/c57c7b37-eef9-4d41-a342-81df871472c2)

LCD exibe: "Luminosidade: Ambiente escuro".

LED verde aceso; buzzer silencioso.

3. Cenário – Temperatura alta (> 15 °C)
![image](https://github.com/user-attachments/assets/b6c27e5a-f44d-4cd8-8322-117ca7f79928)

LCD exibe: "Temp. Alta 24,8 °C".

LED amarelo aceso; buzzer ativo.

4. Cenário – Umidade alta (> 70 %)
![image](https://github.com/user-attachments/assets/22058985-9469-47c2-b0bd-779ea82b742d)
LCD exibe: "Umidade Alta 90 %".

LED vermelho aceso; buzzer ativo.


🛠️ Dificuldades encontradas e soluções

Desafio

Solução

Ruído nas leituras

Média de 5 amostras e delay(100).

Conflito de saídas

Prioridade Temp > Umid > Luz.

Estabilidade do DHT11

Aguardar 2 s no setup() + checar isnan().

Contraste do LCD

Potenciômetro de 10 kΩ em V0.

