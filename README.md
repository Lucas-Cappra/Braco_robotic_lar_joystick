==========================================
==== EXPLICAÇÃO RELACIONADO AO CIRCUITO ====
==========================================

**Configuração do circuito:**
De forma mais objetiva, as conexões relacionadas aos dois módulos serão ditas separadamente. 

![Imagem sobre lpaca PCA9685](https://github.com/Lucas-Cappra/Braco_robotic_lar_joystick/assets/108031562/fedc4245-7ee5-4acc-aebf-09f8122d877d)
=== Módulo PCA9685 ===

*Config do PCA:*
1) Jumper laranja -> terminais (GND + OE) para o GND do Arduíno
2) Jumper Cinza -> terminais SCL para A5 do Arduíno
3) Jumper Branco -> terminais SDA para A4 do Arduíno
4) Jumper Vermelhho -> terminais VCC para VCC do Arduíno

![Imagem sobre Circuito](https://github.com/Lucas-Cappra/Braco_robotic_lar_joystick/assets/108031562/33964546-cdc7-4dae-9828-7732c26ba123)
=== Circuito completo da placa ===


*Config dos módulos:*
1) Jumper Branco -> VCC para VCC do Arduino
2) Jumper Cinza -> GND para GND do Arduino

 *Módulo 2:*
1) Jumper Amarelo -> VRx do módulo 2 para A0 do arduino
2) Jumper Verde -> VRy do módulo 2 para A1 do arduino
3) Jumper Verde afastado -> Switch do módulo 2 para porta digital do arduino

 *Módulo 1:*
1) Jumper Roxo -> VRx do módulo 1 para A2 do arduino
2) Jumper Azul -> VRy do módulo 1 para A3 do arduino
3) Jumper Cinza afastado -> Switch do módulo 1 para porta digital do arduino

NOTA: Os GND1 e GND2 são juntos para conectarem juntos ao arduíno como um para GND. O mesmo para VCC1 e VCC2 para VCC

![Imagem sobre Modolos analogicos](https://github.com/Lucas-Cappra/Braco_robotic_lar_joystick/assets/108031562/db9be6e3-368b-4ff6-968c-c23cb0e2d47f)
=== Imagem sobre Modolos analogicos ===


![Imagem atras do circuito modulos analogicos](https://github.com/Lucas-Cappra/Braco_robotic_lar_joystick/assets/108031562/fec68529-8251-44ad-b5ab-1071036ff41d)
=== Imagem atras do circuito modulos analogicos ===

/// Testar pinagem com https://www.aranacorp.com/en/using-a-pca9685-module-with-arduino/ ///

==========================================
==== EXPLICAÇÃO RELACIONADO AO CÓDIGO ====
==========================================

Este código é destinado a controlar servos motorizados com base na entrada de dois joysticks analógicos. Aqui está um resumo explicativo do código.

1. Inclusão de bibliotecas:
   - O código começa incluindo as bibliotecas Wire.h e Adafruit_PWMServoDriver.h, que são usadas para a comunicação I2C e controle dos servos, respectivamente.

2. Definição de pinos:
   - Os pinos analógicos para os joysticks são definidos como constantes, identificando as entradas analógicas dos dois eixos (X e Y) de dois joysticks.

3. Declaração de variáveis:
   - Variáveis são definidas para controlar as posições dos servos e os valores alvo para cada servo. Também há constantes para suavização de leituras analógicas.

4. Função de configuração (setup):
   - Define o modo dos pinos dos joysticks como entrada.
   - Inicializa a biblioteca Adafruit_PWMServoDriver, configurando a frequência do oscilador e a frequência do PWM.
   - Inicializa a comunicação serial a 9600 bps.
   - Chama a função "inicialServoPosition" para definir as posições iniciais dos servos.

5. Função "inicialServoPosition":
   - Define posições iniciais para os quatro servos conectados.

6. Função "updateServos":
   - Suaviza as leituras dos joysticks analógicos, calculando médias ponderadas das leituras brutas.
   - Mapeia os valores suavizados para ângulos de servo, com algumas considerações para limites e zonas mortas.
   - Atualiza os valores alvo dos servos, suavizando-os e ajustando para limites.
   - Finalmente, envia os valores alvo para os servos usando a biblioteca Adafruit_PWMServoDriver.

7. Função "loop":
   - Chama a função "updateServos" para atualizar continuamente as posições dos servos.
   - Introduz um pequeno atraso para evitar atualizações muito frequentes.


Observações:
   As funções que garantem a função de limitação são relacionadas diretamente aos limites de cada motor, sendo limitados manualmente, protejendo dos limites da mesa principalmente durante a inicialização.

Em resumo, o código recebe leituras dos joysticks analógicos, suaviza essas leituras, mapeia-as para ângulos de servo e controla os servos de acordo com os valores alvo. Isso permite controlar a posição dos servos com os joysticks de forma suave e precisa.
