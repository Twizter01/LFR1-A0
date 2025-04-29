# LFR1-A0
Robot Seguidor de Luz

# 🤖 Robot Seguidor y Escondedor de Luz con Modo de Bajo Consumo

Este proyecto de Arduino permite construir un robot autónomo que **persigue** o **evita la luz** utilizando fotorresistencias (LDRs), motores DC y control mediante botones. Incluye además un **modo de bajo consumo (sleep)** para ahorrar energía.

## 🚀 Características

- 🔄 **Dos modos de funcionamiento:**
  - **PERSEGUIR**: el robot se mueve hacia fuentes de luz.
  - **ESCONDER**: el robot se aleja de fuentes de luz.

- 💤 **Modo Dormir**: desactiva los motores y reduce el consumo energético del Arduino.

- ⚡ Control mediante **interrupciones**:
  - Botón para iniciar/detener el sistema.
  - Botón para cambiar de modo.

---

## 🔧 Componentes Necesarios

| Componente                | Cantidad |
|--------------------------|----------|
| Arduino Uno/Nano         | 1        |
| Motores DC               | 2        |
| Driver L298N o similar   | 1        |
| Fotorresistencias (LDR)  | 2        |
| Pulsadores               | 2        |
| Resistencias (10kΩ)      | 2        |
| Cables / Protoboard      | Varios   |

---

## 🔌 Conexiones

### 🟢 Entradas

| Pin Arduino | Componente                     |
|-------------|--------------------------------|
| D2          | Botón de inicio/parada         |
| D3          | Botón para cambiar de modo     |
| A2          | LDR derecho                     |
| A3          | LDR izquierdo                   |

### 🔴 Salidas

| Pin Arduino | Función                         |
|-------------|---------------------------------|
| D4, D5      | Motor derecho (IN1, IN2)        |
| A0, A1      | Motor izquierdo (IN3, IN4)      |
| D7          | Habilitación del driver (EN)    |
| LED_BUILTIN | Indicador de estado             |

---

## ⚙️ Funcionamiento

1. **Modo Activo**:
   - El robot lee los valores de luz desde dos LDRs.
   - Dependiendo del modo:
     - **PERSEGUIR**: se dirige hacia la luz.
     - **ESCONDER**: se aleja de la luz.

2. **Modo Dormir**:
   - Al presionar el botón de inicio/parada, el robot se apaga (modo dormir).
   - Desactiva el ADC y entra en estado de bajo consumo.
   - Se puede reactivar con el mismo botón.

---

## 📖 Modo de Uso

1. Presiona el botón de **inicio/parada (D2)** para activar el robot.
2. Presiona el botón de **modo (D3)** para cambiar entre PERSEGUIR y ESCONDER.
3. El robot reacciona a la luz ambiental según el modo activo.
4. Vuelve a presionar el botón de inicio/parada para detenerlo y ahorrar energía.

---

## 🧪 Salida Serial (para depuración)

Conecta el Arduino al monitor serial (115200 baudios) para ver:



---

## 📝 Notas

- La sensibilidad a la luz puede ajustarse cambiando el umbral `> 800` en el código.
- Los motores se detienen automáticamente si no hay suficiente luz.
- El sistema usa interrupciones para una respuesta rápida y eficiente.

---

## 🛠️ Mejoras Futuras

- Agregar sensores ultrasónicos para evitar obstáculos.
- Añadir zumbador para alertas de estado.
- Incorporar indicadores LED para el modo activo.

---

## 📄 Licencia

Este proyecto es de código abierto y puedes modificarlo libremente para uso educativo o personal.
