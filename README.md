/// UPDATE :: 11/10/23 - 15:00

# Braco_robotic_lar

#include <Wire.h>
#include <Adafruit_PWMServoDriver.h>

#define JoyStick_X_1 A0 // Analog Pin  X_1
#define JoyStick_X_2 A2 // Analog Pin  X_2
#define JoyStick_Y_1 A1 // Analog Pin  Y_1
#define JoyStick_Y_2 A3 // Analog Pin  Y_2

int pos0 = 102;
int pos180 = 512;
int pos = 0;

Adafruit_PWMServoDriver servos = Adafruit_PWMServoDriver(0x40);

// Valores alvo dos servos
int targetServo0 = 60;
int targetServo1 = 50;
int targetServo2 = 70;
int targetServo3 = pos0;

// Constantes para suavização
const float smoothingFactor = 0.3; // Fator de suavização (ajustável)
int smoothedX1 = 0;
int smoothedX2 = 0;
int smoothedY1 = 0;
int smoothedY2 = 0;

void setup() {
    pinMode(JoyStick_X_1, INPUT);
    pinMode(JoyStick_Y_1, INPUT);
    pinMode(JoyStick_X_2, INPUT);
    pinMode(JoyStick_Y_2, INPUT);
    
    servos.begin();
    servos.setOscillatorFrequency(27000000);
    servos.setPWMFreq(50);
    Serial.begin(9600);
    inicialServoPosition();
}

void inicialServoPosition () {
    servos.setPWM(0, 0, pos0);
    delay(200);
    servos.setPWM(1, 0, pos0);
    delay(200);
    servos.setPWM(2, 0, pos0);
    delay(200);
    servos.setPWM(3, 0, pos0);
}

void updateServos() {
    // Suavização dos valores dos joysticks
    int rawX1 = analogRead(JoyStick_X_1);
    int rawX2 = analogRead(JoyStick_X_2);
    int rawY1 = analogRead(JoyStick_Y_1);
    int rawY2 = analogRead(JoyStick_Y_2);
    
    smoothedX1 = (int)(smoothedX1 * (1.0 - smoothingFactor) + rawX1 * smoothingFactor);
    smoothedX2 = (int)(smoothedX2 * (1.0 - smoothingFactor) + rawX2 * smoothingFactor);
    smoothedY1 = (int)(smoothedY1 * (1.0 - smoothingFactor) + rawY1 * smoothingFactor);
    smoothedY2 = (int)(smoothedY2 * (1.0 - smoothingFactor) + rawY2 * smoothingFactor);
    
    // Mapeamento dos valores suavizados para ângulos dos servos
    int angleServo0 = map(smoothedY1, 0, 1023, 90, -90);
    if(angleServo0 <= 2 && angleServo0 >= -2){
      angleServo0 = 0;
    }
    int angleServo1 = map(smoothedX2, 0, 1023, -90, 90);
    if(angleServo1 <= 10 && angleServo1 >= -10){
      angleServo1 = 0;
    }
    int angleServo2 = map(smoothedX1, 0, 1023, -90, 90);
    /// Deadzone
    if(angleServo2 <= 10 && angleServo2 >= -10){
      angleServo2 = 0;
    }
    int angleServo3 = map(smoothedY2, 0, 1023, -90, 90);
    if(angleServo3 <= 2 && angleServo3 >= -2){
      angleServo3 = 0;
    }
    
    // Definir valores alvo dos servos com suavização
    targetServo0 = (int)(targetServo0 + angleServo0 * smoothingFactor);

    /// Garante a condicao de limitacao
    if(targetServo0 <= 140 && targetServo0 >= 28){
      targetServo0 = (int)(targetServo0 + angleServo0 * smoothingFactor);
    } else { 
        do{
          --targetServo0;
        } while (targetServo0 > 140);
        do{
          ++targetServo0;
        } while (targetServo0 < 28);
    }

    targetServo1 = (int)(targetServo1 + angleServo1 * smoothingFactor);

      Serial.print(" targetServo1  ");
      Serial.println(targetServo1);
    /// Garante a condicao de limitacao
    if(targetServo1 <= 100 && targetServo1 >= 10){
      targetServo1 = (int)(targetServo1 + angleServo1 * smoothingFactor);
    } else { 
        do{
          --targetServo1;
        } while (targetServo1 > 100 );
        do{
          ++targetServo1;
        } while (targetServo1 < 10);
    }
    
    targetServo2 = (int)(targetServo2 + angleServo2 * smoothingFactor);

    /// Garante a condicao de limitacao
    if(targetServo2 <= 100 && targetServo2 >= 10){
      targetServo2 = (int)(targetServo2 + angleServo2 * smoothingFactor);
    } else { 
        do{
          --targetServo2;
        } while (targetServo2 > 100 );
        do{
          ++targetServo2;
        } while (targetServo2 < 10);
    }
    
    targetServo3 = (int)(targetServo3 + angleServo3 * smoothingFactor);

    /// Garante a condicao de limitacao
    if(targetServo3 <= 100 && targetServo3 >= 10){
      targetServo3 = (int)(targetServo3 + angleServo3 * smoothingFactor);
    } else { 
        do{
          --targetServo3;
        } while (targetServo3 > 100 );
        do{
          ++targetServo3;
        } while (targetServo3 < 10);
    }
    
    // Enviar valores alvo para os servos
    servos.setPWM(0, 0, map(targetServo0, 0, 180, pos0, pos180));
    servos.setPWM(1, 0, map(targetServo1, 0, 180, pos0, pos180));
    servos.setPWM(2, 0, map(targetServo2, 0, 180, pos0, pos180));
    servos.setPWM(3, 0, map(targetServo3, 0, 180, pos0, pos180));
}

void loop() {
    updateServos();

    delay(100); // Adicione um pequeno atraso para evitar atualizações muito frequentes
}
