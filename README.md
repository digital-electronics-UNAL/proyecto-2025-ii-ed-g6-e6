[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=22019602&assignment_repo_type=AssignmentRepo)
# Proyecto final - Electrónica Digital 1 - 2025-II

# Integrantes

Cristian Daniel Silva Rodriguez
# Nombre del proyecto
 
Farma-Synchro: Sistema Inteligente de Dispensación de Medicamentos

# Documentación
Debido a las limitaciones de la herramienta que impiden adjuntar archivos o carpetas directamente, comparto a continuación un enlace de acceso a una carpeta en Google Drive que contiene toda la documentación completa del proyecto:

https://drive.google.com/drive/folders/1GaICHaaQqxpLlJLLew8hEMC9onwoEgyb?usp=sharing

## Descripción de la arquitectura

es un sistema de control automatizado que coordina dos servomotores (MG996R y SG90) en respuesta a señales de cuatro sensores TCRT5000. La arquitectura sigue un diseño síncrono basado en máquina de estados, donde todas las operaciones están sincronizadas por un reloj principal de 50 MHz. El sistema presenta tres modos operativos principales: rotación continua del MG996R, secuencia controlada del SG90 cuando se detectan objetos, y un estado de reset permanente que detiene todas las operaciones.

La etapa de entrada procesa las señales de los cuatro sensores invirtiendo Cada señal pasa por un circuito de debouncing digital que implementa un filtro anti-rebote con un período de 50 ms, eliminando fluctuaciones transitorias y garantizando detecciones estables. El sistema diferencia entre los sensores 1-3 que activan la secuencia y el sensor 4 que funciona como reset global, procesándolos con contadores de debouncing independientes para mayor robustez.

Máquina de Estados Finitos 
Estado 0: El servomotor MG996R gira continuamente mientras el SG90 permanece en posición de 0°. El sistema monitorea constantemente los sensores 1-3 y el sensor de reset, priorizando el reset sobre la activación normal.

Estado 1: Cuando se detecta cualquiera de los sensores 1-3, el MG996R se detiene y el SG90 ejecuta una secuencia preprogramada: se mueve a 90°, espera un segundo, regresa a 0° y espera otro segundo antes de retomar el estado inicial.

Estado 2: Estado de bloqueo permanente activado por el sensor de reset, donde ambos servomotores se detienen completamente sin posibilidad de recuperación automática.

El sistema implementa un generador PWM compartido para ambos servomotores, utilizando un contador común que produce una señal de período 20 ms (50 Hz). Para el MG996R, el ancho de pulso varía entre 1.2 ms (rotación continua) y 1.5 ms (posición detenida), mientras que el SG90 utiliza pulsos entre 1 ms (0°) y 2 ms (90°). Las salidas PWM se generan comparando el contador principal con los valores de ancho de pulso configurados para cada motor, optimizando el uso de recursos al emplear lógica compartida.


## Diagramas de la arquitectura
<img width="8785" height="8920" alt="Untitled diagram-2025-12-10-131716" src="https://github.com/user-attachments/assets/a8e1f3b5-d3e2-431e-8317-7f134fc4a51a" />

<img width="6040" height="3761" alt="Untitled diagram-2025-12-10-130017" src="https://github.com/user-attachments/assets/6f7a7b79-0288-4045-89b2-950387ebecf3" />

## Simulaciones}
mg996
<img width="1141" height="432" alt="image" src="https://github.com/user-attachments/assets/69ea50e7-ca9b-4ccd-816c-36115eebf6e5" />

Simulación de la Generación de Señal PWM se visualiza el funcionamiento del módulo encargado de controlar la posición de los servomotores del pastillero. Las gráficas muestran cómo el contador principal (pwm_counter) incrementa linealmente hasta alcanzar el valor del periodo establecido de 20 ms, momento en el cual se reinicia para crear una frecuencia de 50 Hz. Se observa claramente la comparación entre este contador y el ancho de pulso deseado (pulse_width), lo que genera una señal cuadrada de salida con un ciclo de trabajo variable. Esta simulación valida que el sistema es capaz de generar los tiempos precisos necesarios para que el servomotor SG90 abra y cierre la compuerta del dispensador, o para que el motor MG996R rote a la velocidad adecuada.

sg90
<img width="1148" height="436" alt="image" src="https://github.com/user-attachments/assets/94eb7125-8524-4392-8cf9-fe90954a107e" />

tcrt500
<img width="1145" height="289" alt="image" src="https://github.com/user-attachments/assets/3cf85eea-2f2a-461f-90d2-74154ed9db7a" />
Para controlar el movimienmto de los servomotores el los sensores tcr500o se encargan de esto para garantizar la confiabilidad, el sistema incorpora un módulo de antirrebote que filtra el ruido electromecánico de los sensores. La simulación correspondiente muestra cómo una señal de entrada ruidosa, con oscilaciones rápidas entre 0 y 1, es limpiada mediante un filtro temporal. Un contador se incrementa solo cuando la entrada se mantiene estable durante un tiempo mínimo predefinido, asegurando que los cambios de estado detectados por la lógica de control sean válidos y estables, previniendo así activaciones insesarias.

conjunto 

<img width="1555" height="916" alt="image" src="https://github.com/user-attachments/assets/ccc6859e-b48c-4bcc-98e2-b9013a502919" />

Simulación del Sistema General representa la integración de todos los módulos operando en conjunto para la lógica del pastillero. Se observa la transición de estados del sistema : inicialmente, el motor (pwm_mg996r) de la ruleta  recibe una señal de movimiento continuo para buscar el siguiente compartimento. En el momento en que las señales de los sensores (sensor1-4) son validadas por el filtro antirrebote, el sistema cambia de estado, deteniendo inmediatamente el motor del carrusel y activando la secuencia del motor dispensador (pwm_sg90). Esta gráfica confirma la correcta sincronización entre la detección de la posición de cada casilla y la acción mecánica de entrega, asegurando que los motores nunca operen de forma conflictiva.

## Evidencias de implementación

Debido a las limitaciones de la herramienta que impiden incrustar contenido multimedia directamente, adjunto a continuación un enlace a un video demostrativo donde se muestra la funcionalidad operativa completa del proyecto

[https://youtube.com/shorts/Uo1c1HRO6AU?feature=share](https://youtube.com/shorts/965AU6X6AMk?si=6iji46c8U63uGesC)
