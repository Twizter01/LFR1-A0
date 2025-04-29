#include <avr/sleep.h>

/* Variables globales utilizadas en las interrupciones */
volatile bool estadoModoDormir = false;
volatile bool estadoModoPersecucion = true; // Activando modo ESCONDER si es "false"

/* Constantes utilizadas para los pines de Arduino */
// Entradas
const byte pinBotonInicioParo = 2;
const byte pinBotonPerseguirEsconder = 3;
const byte pinFotoResDer = A2;
const byte pinFotoResIzq = A3;

// Salidas
const byte pinHabilitarConductor = 7;
const byte pinMotorDer[] = {4, 5};  // D4, D5
const byte pinMotorIzq[] = {A0, A1}; // A0, A1

void setup()
{
  /* Configuración de pines de salida */
  pinMode(pinHabilitarConductor, OUTPUT);
  pinMode(LED_BUILTIN, OUTPUT);
  
  pinMode(pinMotorIzq[0], OUTPUT);
  pinMode(pinMotorIzq[1], OUTPUT);
  pinMode(pinMotorDer[0], OUTPUT);
  pinMode(pinMotorDer[1], OUTPUT);
  
  /* Configuración de pines de entrada */
  pinMode(pinFotoResDer, INPUT_PULLUP);
  pinMode(pinFotoResIzq, INPUT_PULLUP);
  pinMode(pinBotonInicioParo, INPUT_PULLUP);
  pinMode(pinBotonPerseguirEsconder, INPUT_PULLUP);
  
  /* Configuración de interrupciones externas */
  attachInterrupt(digitalPinToInterrupt(pinBotonInicioParo), botonInicioParoPresionado, FALLING);
  attachInterrupt(digitalPinToInterrupt(pinBotonPerseguirEsconder), botonPerseguirEsconderLiberado, RISING);
  
  Serial.begin(115200);
}

void loop()
{
  if (!estadoModoDormir) // Iniciar
  {
    int valorFotoResDer = 1023 - analogRead(pinFotoResDer);
    int valorFotoResIzq = 1023 - analogRead(pinFotoResIzq);
    int diferenciaFotoRes = abs(valorFotoResIzq - valorFotoResDer);

    Serial.print("LDR Izquierdo: "); Serial.print(valorFotoResIzq);
    Serial.print("\tLDR Derecho: "); Serial.print(valorFotoResDer);
    Serial.print("\tDiferencia LDR: "); Serial.print(diferenciaFotoRes);
    Serial.print("\tModo: ");
    
    digitalWrite(pinHabilitarConductor, HIGH);
    
    if (!estadoModoPersecucion) // MODO PERSEGUIR
    {
      Serial.print("PERSEGUIR\tAcción: ");
      
      if (diferenciaFotoRes <= 10 && valorFotoResIzq > 800 && valorFotoResDer > 800)
        avanzar();
      else if (valorFotoResIzq > valorFotoResDer && valorFotoResIzq > 800)
        girarIzquierda();
      else if (valorFotoResDer > valorFotoResIzq && valorFotoResDer > 800)
        girarDerecha();
      else
        detener();
    }
    else // MODO ESCONDER
    {
      Serial.print("ESCONDER\tAcción: ");
      
      if (diferenciaFotoRes <= 10 && valorFotoResIzq > 800 && valorFotoResDer > 800)
        retroceder();
      else if (valorFotoResIzq > valorFotoResDer && valorFotoResIzq > 800)
        girarDerechaAtras();
      else if (valorFotoResDer > valorFotoResIzq && valorFotoResDer > 800)
        girarIzquierdaAtras();
      else
        detener();
    }
  }
  else // Detener
  {
    digitalWrite(pinHabilitarConductor, LOW);
    Serial.println("Durmiendo...");
    entrarEnModoDormir();
  }
}

void entrarEnModoDormir()
{
  byte adcsra = ADCSRA;
  ADCSRA = 0; // Deshabilitar ADC
  set_sleep_mode(SLEEP_MODE_IDLE);
  cli(); // Deshabilitar interrupciones
  sleep_enable();
  sleep_bod_disable();
  sei(); // Habilitar interrupciones
  sleep_cpu(); // Entrar en modo dormir
  sleep_disable(); // Despertar
  ADCSRA = adcsra; // Restaurar configuración ADC
}

void botonInicioParoPresionado()
{
  estadoModoDormir = !estadoModoDormir;
}

void botonPerseguirEsconderLiberado()
{
  estadoModoPersecucion = !estadoModoPersecucion;
}

void detener()
{
  digitalWrite(pinMotorIzq[0], LOW);
  digitalWrite(pinMotorIzq[1], LOW);
  digitalWrite(pinMotorDer[0], LOW);
  digitalWrite(pinMotorDer[1], LOW);
  Serial.println("Detenido");
}

void avanzar()
{
  digitalWrite(pinMotorIzq[0], HIGH);
  digitalWrite(pinMotorIzq[1], LOW);
  digitalWrite(pinMotorDer[0], LOW);
  digitalWrite(pinMotorDer[1], HIGH);
  Serial.println("Avanzar");
}

void retroceder()
{
  digitalWrite(pinMotorIzq[0], LOW);
  digitalWrite(pinMotorIzq[1], HIGH);
  digitalWrite(pinMotorDer[0], HIGH);
  digitalWrite(pinMotorDer[1], LOW);
  Serial.println("Retroceder");
}

void girarDerecha()
{
  digitalWrite(pinMotorIzq[0], HIGH);
  digitalWrite(pinMotorIzq[1], LOW);
  digitalWrite(pinMotorDer[0], LOW);
  digitalWrite(pinMotorDer[1], LOW);
  Serial.println("Girar Derecha");
}

void girarDerechaAtras()
{
  digitalWrite(pinMotorIzq[0], LOW);
  digitalWrite(pinMotorIzq[1], HIGH);
  digitalWrite(pinMotorDer[0], LOW);
  digitalWrite(pinMotorDer[1], LOW);
  Serial.println("Girar Derecha Atrás");
}

void girarIzquierda()
{
  digitalWrite(pinMotorIzq[0], LOW);
  digitalWrite(pinMotorIzq[1], LOW);
  digitalWrite(pinMotorDer[0], LOW);
  digitalWrite(pinMotorDer[1], HIGH);
  Serial.println("Girar Izquierda");
}

void girarIzquierdaAtras()
{
  digitalWrite(pinMotorIzq[0], LOW);
  digitalWrite(pinMotorIzq[1], LOW);
  digitalWrite(pinMotorDer[0], HIGH);
  digitalWrite(pinMotorDer[1], LOW);
  Serial.println("Girar Izquierda Atrás");
}
