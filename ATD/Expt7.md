# Radiation Pattern for two-isotropic point sources and N-isotropic point sources for Broadside and End-fire array using MATLAB
## MATLAB Code:
```matlab
clc; clear all; close all;
theta = 0:0.01:2*pi;
el = cos((pi/2)*cos(theta));
polar(theta, abs(el)/max(el));
title('radiation pattern of antenna');
figure;
plot(theta*180/pi, abs(el));
xlabel('angle in degrees'); ylabel('absolute gain');
grid on;
title('gain of radiation pattern');
```
## Cases
- $\delta = 0$ and d = $\lambda /2$
- $\delta = \pi$ and d = $\lambda /2$
- $\delta = \pi /2$ and d = $\lambda /4$
