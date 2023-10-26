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
