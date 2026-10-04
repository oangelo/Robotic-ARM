# Robotic-ARM

Braço robótico **4 DOF** impresso em 3D, acionado por **BLDC outrunner + FOC** (MKS XDRIVE Mini / ODrive), controlado via **ROS2 + MoveIt2**.

> Objetivo: gastar mais e ter algo útil — aprender controle fechado (FOC) E construir um braço que faz trabalho de verdade (pick-and-place de peças/parafusos por visão).

Este repo retoma o antigo repositório (era um esqueleto Arduino de 2019, sem nada útil) e o transforma no projeto do braço robótico BLDC/FOC.

## Escopo
- **Hardware:** motor Eaglepower 8308 KV180 (ombro/base) + MKS XDRIVE Mini (driver FOC, encoder AS5047P onboard); juntas menores em direção ao pulso.
- **Estrutura:** impressa em 3D (referência: projeto dARM — STL + BOM + ROS2).
- **Software:** ROS2 + ros2_control + MoveIt2. Fase 1 (1 junta) pode usar só ODrive/Arduino com ESP32.
- **Missão:** organizar peças/parafusos por visão (OpenCV + AprilTag). Fora de escopo: dobra de roupa (deformável — pesquisa), força/sensibilidade fina.

## Estado
Documento de planejamento e decisões em [`plano.md`](plano.md) (a fonte de verdade do projeto). Compra da Fase 1 ainda não realizada.

## Referências
- dARM (braço impresso BLDC/ODrive): github.com/JesseDarr/dARM · dARM_ros2
- ODrive ROS2: github.com/odriverobotics/ros_odrive
- MKS XDRIVE Mini setup: github.com/justlovescience/MKS-XDRIVE-MINI
- MoveIt2: github.com/moveit/moveit2