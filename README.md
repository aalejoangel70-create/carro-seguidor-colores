# 🚗 Carro Seguidor de Líneas de Colores — Proyecto Final Sistemas Digitales

> **Fundación Universitaria Compensar · Telecomunicaciones**  
> Curso: Sistemas Digitales · Docente: Diego Alejandro Barragán Vargas

---

## ¿De qué va esto?

Honestamente, cuando nos dijeron que el proyecto final era construir un carro robótico que detectara colores y lo controláramos por voz desde una app web... pensamos que era demasiado. Spoiler: sí lo era. Pero lo logramos.

Este repositorio documenta **todo** el proceso: desde las primeras soldaduras (y quemadas de dedos) hasta la app funcionando en Streamlit con chatbot incluido. Si estás leyendo esto y eres estudiante de semestres futuros, ojalá este repo te ahorre las 3 horas que nosotros perdimos tratando de entender por qué el HC-05 no emparejaba. 

---

## 🎯 Objetivo del Proyecto

Diseñar e implementar un sistema embebido basado en Arduino que:

- ✅ Siga líneas de **5 colores distintos** (negro, blanco, rojo, verde, azul)
- ✅ Transmita el color detectado en **tiempo real** a una app web en Streamlit
- ✅ Incluya un **chatbot conversacional** que explique el proyecto
- ✅ Reciba **comandos de voz** para controlar el movimiento del carro (arriba, abajo, derecha, izquierda)
- ✅ Comunicación bidireccional Arduino ↔ PC vía USB o Bluetooth

---

## 📸 Así quedó el bicho

> Aquí está el proceso completo, desde el chasis pelado hasta el carro listo para rodar.

### Fase 1 — Ensamblaje inicial del chasis

En este punto aún éramos optimistas. El chasis de MDF llegó en piezas y tardamos más en armarlo que en programar la mitad del código.

| Vista frontal con sensor ultrasónico | Vista superior con Arduino |
|--------------------------------------|---------------------------|
| ![Carro ensamblado vista frontal](WhatsApp%20Image%202026-05-29%20at%205.02.32%20PM.jpeg) | ![Carro vista superior](WhatsApp%20Image%202026-05-29%20at%205.02.32%20PM%20(1).jpeg) |

### Fase 2 — Integración de componentes en el laboratorio

Acá empezó el caos bonito. Cables por todos lados, el profesor mirando desde lejos con cara de "van a quemarlo todo", y nosotros tratando de descifrar el datasheet del L298N a las 10pm.

| Revisando conexiones del driver de motores | Vista completa del cableado |
|--------------------------------------------|-----------------------------|
| ![Laboratorio vista trasera](WhatsApp%20Image%202026-05-29%20at%205.02.32%20PM%20(2).jpeg) | ![Vista completa](WhatsApp%20Image%202026-05-29%20at%205.02.33%20PM.jpeg) |

### Fase 3 — Cableado fino y sensor de color

La parte más delicada. El TCS3200/HC-05 tiene pines muy juntos y en este punto ya teníamos el multi­metro como mejor amigo.

| Ajuste de pines del sensor | Conexiones del módulo Bluetooth HC-05 |
|----------------------------|---------------------------------------|
| ![Ajuste sensor](WhatsApp%20Image%202026-05-29%20at%205.02.33%20PM%20(1).jpeg) | ![Bluetooth HC-05 y sensor de color](WhatsApp%20Image%202026-05-29%20at%205.02.33%20PM%20(2).jpeg) |

---

## 🧰 Componentes utilizados

| Componente | Especificación | ¿Para qué sirve? |
|---|---|---|
| **Arduino Uno** | ATmega328P | El cerebro de todo. Recibe datos del sensor y controla los motores |
| **Sensor de color** | TCS3200 / TCS34725 | Detecta el color de la línea bajo el carro |
| **Chasis** | 4WD MDF | La base física del carro — lo armamos nosotros |
| **Motores DC** | 3–6V con ruedas amarillas | La tracción, sin ellos sería solo una tabla con cables |
| **Driver de motores** | L298N | Controla dirección y velocidad de cada motor |
| **Batería** | 9V / Pack Li-ion 7.4V | Alimenta todo el sistema |
| **Módulo Bluetooth** | HC-05 (opcional) | Comunicación inalámbrica con la PC |
| **Sensor ultrasónico** | HC-SR04 | Detección de obstáculos (feature extra 👀) |
| **Protoboard + cables** | Jumpers M-M, M-H | Las conexiones que mantienen todo unido |

---

## 🗂️ Estructura del repositorio

```
📦 carro-seguidor-colores/
├── 📁 arduino/
│   ├── carro_seguidor.ino        ← Código principal del Arduino
│   └── calibracion_colores.ino  ← Sketch para calibrar el sensor TCS3200
├── 📁 streamlit_app/
│   ├── app.py                   ← Dashboard principal en Streamlit
│   ├── chatbot.py               ← Módulo del chatbot conversacional
│   ├── serial_bridge.py         ← Comunicación serial Arduino ↔ Python
│   └── voice_commands.py        ← Reconocimiento de voz (comandos de movimiento)
├── 📁 images/
│   ├── foto1.jpeg               ← Fotos del proceso
│   ├── foto2.jpeg
│   ├── foto3.jpeg
│   ├── foto4.jpeg
│   ├── foto5.jpeg
│   └── foto6.jpeg
├── 📁 docs/
│   └── PROYECTO_FINAL.pdf       ← Guía original del proyecto
├── requirements.txt             ← Dependencias de Python
└── README.md                    ← Esto que estás leyendo
```

---

## ⚙️ ¿Cómo funciona todo junto?

El flujo es más sencillo de lo que parece una vez que lo ves completo:

```
[Línea de color en el piso]
        ↓
[Sensor TCS3200 detecta el color]
        ↓
[Arduino Uno procesa la lectura]
        ↓  ←— también recibe comandos de movimiento
[Comunicación Serial / Bluetooth HC-05]
        ↓
[Python lee el puerto COM / /dev/rfcomm0]
        ↓
[Streamlit muestra el color en tiempo real]
        ↓
[Chatbot responde preguntas sobre el proyecto]
[Reconocimiento de voz envía comandos al Arduino]
```

---

## 💻 Instalación y uso

### 1. Clona el repositorio

```bash
git clone https://github.com/TU_USUARIO/carro-seguidor-colores.git
cd carro-seguidor-colores
```

### 2. Instala las dependencias de Python

```bash
pip install -r requirements.txt
```

El `requirements.txt` incluye:
```
streamlit
pyserial
speechrecognition
anthropic        # Para el chatbot con Claude
pyaudio
```

### 3. Carga el código al Arduino

Abre `arduino/carro_seguidor.ino` en el Arduino IDE y cárgalo al Arduino Uno. Antes de cargar, verifica en la pestaña de calibración cuáles son los valores RGB que entrega el sensor para cada color en tu pista — cada impresión es diferente.

### 4. Conecta el Arduino y corre la app

```bash
# Asegúrate de cambiar el puerto COM en serial_bridge.py
# Windows: COM3, COM4, etc.
# Linux/Mac: /dev/ttyUSB0, /dev/rfcomm0 (Bluetooth)

streamlit run streamlit_app/app.py
```

### 5. ¡Listo!

La app se abre en `http://localhost:8501`. Desde ahí puedes ver el color detectado en tiempo real, hablarle al chatbot y darle comandos de voz al carro.

---

## 🎙️ Comandos de voz disponibles

| Lo que dices | Lo que hace el carro |
|---|---|
| *"Arriba"* / *"Adelante"* | Avanza hacia adelante |
| *"Abajo"* / *"Atrás"* | Retrocede |
| *"Derecha"* | Gira a la derecha |
| *"Izquierda"* | Gira a la izquierda |
| *"Detente"* / *"Para"* | Frena |

---

## 🎨 Colores que detecta el carro

La pista tiene segmentos de 5 colores distintos y el carro identifica en cuál está:

| Color | ¿Qué pasa en Streamlit? |
|---|---|
| ⬛ Negro | Se muestra fondo negro con texto blanco |
| ⬜ Blanco | Fondo blanco, texto oscuro |
| 🔴 Rojo | Alerta visual en rojo |
| 🟢 Verde | Indicador verde activo |
| 🔵 Azul | Panel azul iluminado |

---

## 🤖 El Chatbot

Integramos un chatbot usando la API de Claude (Anthropic) que puede responder preguntas como:

- *"¿Qué componentes usaron?"*
- *"¿Cómo funciona el sensor de color?"*
- *"¿Por qué eligieron el L298N y no el L293D?"*
- *"¿Cómo conectaron el HC-05?"*

El chatbot tiene contexto completo del proyecto, así que puede explicar desde el cableado hasta la arquitectura del software.

---

## 🐛 Problemas que nos encontramos (y cómo los resolvimos)

Porque un README honesto incluye las partes malas también:

**El HC-05 no emparejaba con Windows 11**  
→ Solución: hay que ponerlo en modo AT y cambiar la velocidad de baudios a 9600. Usamos el comando `AT+UART=9600,0,0`.

**El TCS3200 confundía el rojo con el naranja bajo luz fluorescente**  
→ Solución: calibrar los valores en el mismo ambiente donde va a funcionar. Los valores del datasheet son solo un punto de partida.

**El carro se iba recto aunque le mandáramos girar**  
→ Solución: los motores del lado derecho e izquierdo no son idénticos. Tuvimos que ajustar el PWM de cada lado por separado.

**Streamlit se reiniciaba cada vez que llegaba un dato serial**  
→ Solución: `st.session_state` para guardar el estado entre reruns, y threading para la lectura serial en segundo plano.

---

## 📐 Diagrama de conexiones (Arduino)

```
Arduino Uno
├── Pin 2  → TCS3200 S0
├── Pin 3  → TCS3200 S1
├── Pin 4  → TCS3200 S2
├── Pin 5  → TCS3200 S3
├── Pin 6  → TCS3200 OUT
├── Pin 7  → L298N IN1
├── Pin 8  → L298N IN2
├── Pin 9  → L298N IN3 (PWM)
├── Pin 10 → L298N IN4 (PWM)
├── Pin 11 → HC-05 TX
├── Pin 12 → HC-05 RX
├── 5V     → TCS3200 VCC, HC-05 VCC
└── GND    → Todo el GND común
```

---

## 👥 Equipo

Este proyecto fue desarrollado como trabajo final del curso **Sistemas Digitales** en la Fundación Universitaria Compensar.

**Docente:** Diego Alejandro Barragán Vargas — Ingeniero Electrónico, Magíster en Ingeniería, estudiante doctoral en la UDFJC.

---

## 📄 Licencia

MIT — úsalo, modifícalo, mejóralo. Si lo usas para tu propio proyecto, un crédito siempre se agradece 🙌

---

> *"El primer carro que hicimos iba en círculos. El segundo iba en círculos más grandes. Al tercero le pusimos nombre."*  
> — El equipo, semestre 2026-1
