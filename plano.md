# Braço robótico BLDC + FOC — plano de projeto (Angelo)

> Objetivo: gastar mais e ter algo útil — aprender controle fechado (FOC) E construir um braço que faz trabalho de verdade. Alvo: **4 DOF** (base giratória + ombro + cotovelo + pulso), pick-and-place real.
> Criado: 04/10/2026.

## Decisões assentadas
- Ecossistema: **BLDC outrunner + driver FOC (MKS XDRIVE Mini / ODrive) + encoder magnético**.
- **Motor de hoverboard: DESCARTADO para este projeto** (04/10). XDRIVE é p/ outrunner com eixo exposto p/ encoder AS5047P; hub selado de hoverboard → VESC (driver provado p/ isso).
- Referência de braço impresso: **dARM** (JesseDarr) — STL grátis (Printables 1256981, MakerWorld), 6 DOF, 2,2 kg. Impresso em 3D.
- Estrutura: impressa em **PETG/ASA** (não PLA). Rigor nas juntas/rolamentos.
- Software: **ROS2 + ros2_control + MoveIt2 + RViz** (multiplas juntas). Fase 1 (1 junta) pode usar só ODrive/Arduino.

## Compra em FASE (não descer tudo de uma vez)
### Fase 1 — junta forte (base/turntable útil + aprender FOC) ≈ R$785
- Motor **Eaglepower 8308 KV180** — R$369,99 (item 1005005084172325) — mesmo tipo que o dARM usa (segura ~2,2 kg horiz.)
- Driver **MKS XDRIVE Mini** — R$204,69 (item 1005006480243178) — encoder AS5047P onboard, é o "ODrive barato".
- ímã encoder AS5047P (~R$10) + fonte de força dimensionada (+24V) + placa de controle (ESP32 com CAN / USB).
- ⚠️ Não rodar `upgrade` no odrivetool (brica — firmware v0.5.1). CAN conflict: ghost axis1 → listen-only (id 63).

### Fase 2 — cotovelo (médio) ≈ R$200
- Outrunner médio 5010/6354 (~R$150-250) + 1 driver XDRIVE. Confirmar torque p/ segurar antebraço+pulso.

### Fase 3 — pulso (2 pequenos) ≈ R$100
- 2 gimbal pequenos (2804/5010-class) + 2 drivers XDRIVE. Torque de pulso ~0,3-0,6 N·m.

### Orçamento total 4 DOF (motores + drivers) ≈ R$1850-2200
+ estrutura impressa, rolamentos, garra, fonte. 6 DOF completo = sobe pra cima disso (dARM gasta ~US$1600 só em motores+ODrive).

## Links e compras — AliExpress + vídeo (preços de 04/10/2026)
- **Vídeo de referência do driver** (Justlovescience, "Robotics on a Budget" — MKS XDRIVE Mini como ODrive barato): https://youtu.be/yRx7dsJmNvU · GitHub: https://github.com/justlovescience/MKS-XDRIVE-MINI
- **Fase 1 — Motor Eaglepower 8308 KV180** (R$369,99 + ~R$159 impostos ≈ R$530): https://pt.aliexpress.com/item/1005005084172325.html
- **Fase 1 — Driver MKS XDRIVE Mini** (R$204,69 + ~R$51 impostos ≈ R$256): https://pt.aliexpress.com/item/1005006480243178.html
- Alt — ME7010 flat alto torque (R$172,59; fraco p/ ombro — serviria p/ pulso): https://pt.aliexpress.com/item/1005009462970916.html
- Alt — Gimbal Makerbase 2804/5010 + AS5600 (R$46,69; pulso): https://pt.aliexpress.com/item/1005012729644704.html
- Alt — Kit FOC 2804 rotor externo + Hall (R$96,79): https://pt.aliexpress.com/item/1005012849384154.html
- **Impostos:** item abaixo de US$50 = só ICMS ~17%; acima de US$50 = +60% imp. importação (desconto US$30).

## Software stack
- **odrivetool** (config/lookup do motor, pole pairs, encoder calibração).
- ROS2 packages: `odriverobotics/ros_odrive` (`odrive_node` + `odrive_ros2_control`), por USB ou CAN.
- **ros2_control**: controller_manager, joint_state_broadcaster, position controllers.
- **MoveIt2**: URDF → MoveIt Setup Assistant → OMPL planejamento, IK, colisão, RViz.
- Fase 1: `ODriveArduino`/ESP32 — controlar junta por ângulo sem ROS.
- Simulação: Gazebo/ros2_control — validar 4 DOF antes de comprar tudo.

## Referências
- dARM hard: github.com/JesseDarr/dARM · STL: printables.com/model/1256981
- dARM ROS2: github.com/JesseDarr/dARM_ros2
- ODrive ROS2: github.com/odriverobotics/ros_odrive · Factor-Robotics/odrive_ros2_control
- MoveIt2: github.com/moveit/moveit2 · will2022/ros2-moveit-6dof-arm
- Video driver (Justlovescience): github.com/justlovescience/MKS-XDRIVE-MINI

## Próximo passo
- Confirmar compra Fase 1 e listar acessórios exatos (ímã, fonte, ESP32). Cadastrar no lembrete de compras quando fechar.

## Aplicações — escopo realista (04/10, definido com Angelo)
- ✅ **Organizar parafusos/peças**: bandeja → potes, visão (OpenCV + AprilTag/ARUco p/ calibração). Peça rígida em plano conhecido = tarefa realista com 4 DOF. Caso de uso central.
- ✅ Pick-and-place de objetos pequenos/rígidos; mesa giratória/pan-tilt; mini linha de montagem.
- ❌ Dobrar roupas/tecido (deformável — pesquisa de lab, fora do escopo).
- ❌ Manipulação fina com força/sensibilidade (precisa de sensor de força, fora do orçamento).

## Camada de IA (04/10)
- **IA para CONSTRUIR/guia** (URDF, ros2_control, MoveIt, FOC, debug): USAR copiloto LLM (Gemini/ChatGPT/Claude). Guias "LLM + ROS2" (Ollama local) e projeto LAVOOK (braço 4-eixos IA + voz). Prático e imediato.
- **IA para DOBRAR tecido** (Gemini Robotics 2 / ALOHA / SSFold): EXISTE e dobra roupa por voz, mas roda em humanóides de pesquisa (Apptronik Apollo, 09/2025) — NÃO roda em braço impresso caseiro. Deformável segue fora do escopo deste build.

## Sensores (04/10) — o que precisa de verdade
- **Encoder de junta AS5047P** (~14-bit): JÁ onboard no XDRIVE Mini → só colar **ímã diamétrico** no eixo do motor (kit ~R$10/10), 1 por motor.
- **Sensoriamento de corrente/fase**: embarcado no driver (FOC), não compra.
- **1 câmera RGB** (webcam USB Logitech C270 / câmera Pi) + marcadores **AprilTag/ARUco** impressos p/ calibração → destrava pick-and-place por visão (missão do braço).
- **Opcional: micro switch/fim de curso por junta** (~R$2) p/ homing da zero.
- NÃO precisa: IMU, LiDAR, encoder extra, sensor de força.