# Matrice 100

# Matrice 100

{% youtube %}https://www.youtube.com/watch?v=3OGs0ONemGc{% endyoutube %}

![Matrice 100](../pictures/airframes/multicopter/matrice100/Matrice100.jpg)

## Parts List

- [DJI Matrice 100](http://store.dji.com/product/matrice-100) Just ESC''s motors, and frame.

## Motor Connections

![Connections](../pictures/airframes/multicopter/matrice100/Wiring Diagram.jpg)

![Wiring Harness](../pictures/airframes/multicopter/matrice100/WiringHarness.jpg)

![PWM Connections](../pictures/airframes/multicopter/matrice100/PwmInput.jpg)

![Top](../pictures/airframes/multicopter/matrice100/Top.jpg)

![Back](../pictures/airframes/multicopter/matrice100/Back.jpg)

![No Stack](../pictures/airframes/multicopter/matrice100/NoStack.jpg)

![No Top Deck](../pictures/airframes/multicopter/matrice100/NoTopDeck.jpg)

| Output | Rate   | Actuator         |
| ------ | ------ | ---------------- |
| MAIN1  | 400 Hz | Front right, CCW |
| MAIN2  | 400 Hz | Back left, CCW   |
| MAIN3  | 400 Hz | Front left, CW   |
| MAIN4  | 400 Hz | Back right, CW   |
| AUX1   | 50 Hz  | RC AUX1          |
| AUX2   | 50 Hz  | RC AUX2          |
| AUX3   | 50 Hz  | RC AUX3          |

## Parameters

- At high throttle the inner loop causes oscillations with default x quad gains. At low throttle, higher gains give a better response, this suggests that some gain scheduling based on the throttle may improve the overall response and this could be implemented in mc_att_control. For now we will just tune it so that there are no oscillations at low or high throttle, and take the bandwidth hit at low throttle.
  - MC_PITCHRATE_P: 0.05
  - MC_PITCHRATE_D: 0.001
- The battery has 6 cells instead of the default 3
  - BAT_N_CELLS: 6