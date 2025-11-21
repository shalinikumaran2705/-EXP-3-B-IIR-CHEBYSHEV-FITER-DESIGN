# EXP 3 : IIR-CHEBYSHEV-FITER-DESIGN

## AIM: 

 To design an IIR Chebyshev filter  using SCILAB. 

## APPARATUS REQUIRED: 
PC installed with SCILAB. 

## PROGRAM (LPF): 
```
clc ;
close ;
wp=input('Enter the pass band frequency (Radians )= ' );
ws=input('Enter the stop band frequency (Radians )= ' );
alphap=input( ' Enter the pass band attenuation (dB)=' );
alphas=input( ' Enter the stop band attenuation(dB)=' );
T=input('Enter the Value of sampling Time=');
omegap=(2/T)*tan(wp/2);
disp(omegap,'omegap=');
omegas=(2/T)*tan(ws/2);
N=acosh(sqrt(((10^(0.1*alphas))-1)/((10^(0.1*alphap))-1)))/(acosh(omegas/omegap));
disp(N,'N=');
N=ceil(N);
disp(N,'Round off value of N=');
omegac=omegap/(((10^(0.1*alphap)) -1)^(1/(2* N)));
disp(omegac,'omegac=');
Epsilon = sqrt ((10^(0.1*alphap))-1);
disp(Epsilon,'Epsilon=');
[pols ,gn] = zpch1(N, Epsilon,omegap );
disp(gn,'Gain');
disp(pols,'Poles');
hs=poly(gn,'s','coeff')/real(poly(pols,'s'));
disp(hs,'Analog Low pass Chebyshev Filter Transfer function');
z=poly(0,'z');
Hz=horner(hs,(2/ T)*((z -1)/(z+1)));
disp(Hz,'Digital LPF Transfer function H(Z)=');
HW=frmag(Hz,512); 
w=0:%pi/511:%pi;
plot(w/%pi,abs(HW));
xlabel(' Normalized Digital Frequency w');
ylabel('Magnitude ');
title(' Frequency Response of Chebyshev IIR LPF');

```


## PROGRAM (HPF): 
```
clc;
clear;
close;

wp = input('Enter the pass band frequency (Radians)= ');
ws = input('Enter the stop band frequency (Radians)= ');
alphap = input('Enter the pass band attenuation (dB)= ');
alphas = input('Enter the stop band attenuation (dB)= ');
T = input('Enter the value of sampling time=');

// --- Pre-warping analog frequencies ---
omegap = (2/T)*tan(wp/2);
omegas = (2/T)*tan(ws/2);
disp(omegap, 'omegap=');
disp(omegas, 'omegas=');

// --- Filter order ---
N = acosh(sqrt(((10^(0.1*alphas))-1)/((10^(0.1*alphap))-1))) / acosh(omegap/omegas);
disp(N, 'N=');
N = ceil(N);
disp(N, 'Rounded off value of N=');

// --- Cutoff frequency ---
omegac = omegap / (((10^(0.1*alphap))-1)^(1/(2*N)));
disp(omegac, 'omegac=');

Epsilon = sqrt((10^(0.1*alphap))-1);
disp(Epsilon, 'Epsilon=');

// --- Analog low pass prototype ---
[pols, gn] = zpch1(N, Epsilon, omegac);
disp(gn, 'Gain=');
disp(pols, 'Poles=');

// --- Low pass transfer function ---
hs = poly(gn, 's', 'coeff') / real(poly(pols, 's'));
disp(hs, 'Analog Low Pass Chebyshev Filter Transfer Function=');

// --- Convert LPF to HPF: s -> (omegac^2 / s) ---
s = poly(0, 's');
hs_num = coeff(hs.num);
hs_den = coeff(hs.den);

// substitute s -> (omegac^2 / s)
hs_hpf = horner(hs, (omegac^2 / s));
disp(hs_hpf, 'Analog High Pass Chebyshev Filter Transfer Function=');

// --- Bilinear Transform: s = (2/T)*((1 - z^-1)/(1 + z^-1)) ---
z = poly(0, 'z');
Hz = horner(hs_hpf, (2/T)*((1 - z^-1)/(1 + z^-1)));
disp(Hz, 'Digital High Pass Chebyshev Filter Transfer Function H(z)=');

// --- Frequency response ---
HW = frmag(Hz, 512);
w = 0:%pi/511:%pi;
plot(w/%pi, abs(HW));
xlabel('Normalized Digital Frequency (w/pi)');
ylabel('Magnitude');
title('Frequency Response of Chebyshev IIR High Pass Filter');
```



## OUTPUT (LPF) : 

<img width="765" height="572" alt="image" src="https://github.com/user-attachments/assets/2799c447-9681-468c-a75b-e2d2b0e9cafc" />


## OUTPUT (HPF) : 

<img width="813" height="560" alt="image" src="https://github.com/user-attachments/assets/1a207205-3ec9-4c87-b057-675da83836c7" />


## RESULT: 

The design of an IIR Chebyshev filter using SCILAB is sucessfully completed.
