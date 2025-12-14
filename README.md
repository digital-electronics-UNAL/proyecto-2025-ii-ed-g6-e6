[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=22019602&assignment_repo_type=AssignmentRepo)
# Proyecto final - Electrónica Digital 1 - 2025-II

# Integrantes

Cristian Daniel Silva Rodriguez
# Nombre del proyecto
 
Farma-Synchro: Sistema Inteligente de Dispensación de Medicamentos

# Documentación

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
sg90
<img width="1148" height="436" alt="image" src="https://github.com/user-attachments/assets/94eb7125-8524-4392-8cf9-fe90954a107e" />
tcrt500
<img width="1145" height="289" alt="image" src="https://github.com/user-attachments/assets/3cf85eea-2f2a-461f-90d2-74154ed9db7a" />
mg996
<img width="1141" height="432" alt="image" src="https://github.com/user-attachments/assets/69ea50e7-ca9b-4ccd-816c-36115eebf6e5" />
conjunto 
<img width="1555" height="916" alt="image" src="https://github.com/user-attachments/assets/ccc6859e-b48c-4bcc-98e2-b9013a502919" />


## Evidencias de implementación

