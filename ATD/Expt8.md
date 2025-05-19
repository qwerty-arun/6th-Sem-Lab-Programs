# Power Division Characteristics and VSWR of Circulatro and Isolator and verify the S-Matrices
## MATLAB Code
```matlab
clc; close all; clear all;

theta = 0:0.01:2*pi;

L = input('enter the value of L:');

e = (cos(pi*L*cos(theta)) - cos(pi*L)) ./ sin(theta);

subplot(1,2,1);
polar(theta, abs(e)/max(e));
title('Radiation pattern of antenna');

subplot(1,2,2);
plot(theta.*180/pi, abs(e));
xlabel('Angle in degrees');
ylabel('Absolute gain');
grid on;
title('Radiation pattern');
```

## Cases
- L = $\lambda /2$
- L = $\lambda $
- L = $3\lambda /2$
- L = $5\lambda /2$
