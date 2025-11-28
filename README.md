# Frequency-Division-Multiplexing---Modulation-and-Demodulation
# Aim:
To generate an FDM signal by multiplexing multiple baseband message signals on different carrier frequencies, transmit (sum) them, optionally add channel noise, then recover each message by bandpass filtering and coherent demodulation in Python (Google Colab). Observe time & frequency domain signals and measure recovery quality.
# Apparatus Required:
Python libraries: numpy, matplotlib, scipy (scipy.signal)
# Theory:
FDM places different message signals in separate, non-overlapping frequency bands by modulating each message onto a distinct carrier frequency. The multiplexed signal is the sum of all modulated channels. At the receiver, bandpass filters (or tuned filters) isolate each channel; then each isolated carrier is demodulated (coherently multiplied by a synchronized carrier) and low-pass filtered to recover the original baseband.
# Procedure:
1 — Imports and parameters

2 — Create message signals and carriers

3 — Modulate each message (standard AM DSB-SC) and form FDM signal

4 — Frequency domain (spectrum) of FDM signal

5 — (Optional) Add AWGN noise to FDM signal

6 — Receiver: isolate each channel with bandpass filter

7 — Demodulate each isolated channel (coherent) and low-pass filter to recover baseband.

# Code:
````
clc;
clear;
close;

fs = 50000; 
t = 0:1/fs:0.01;

fm = [200, 400, 600, 800, 1000, 1200];  
m1 = sin(2*%pi*fm(1)*t);
m2 = sin(2*%pi*fm(2)*t);
m3 = sin(2*%pi*fm(3)*t);
m4 = sin(2*%pi*fm(4)*t);
m5 = sin(2*%pi*fm(5)*t);
m6 = sin(2*%pi*fm(6)*t);

fc = [4000, 6000, 8000, 10000, 12000, 14000];

c1 = cos(2*%pi*fc(1)*t);
c2 = cos(2*%pi*fc(2)*t);
c3 = cos(2*%pi*fc(3)*t);
c4 = cos(2*%pi*fc(4)*t);
c5 = cos(2*%pi*fc(5)*t);
c6 = cos(2*%pi*fc(6)*t);

s1 = m1 .* c1;
s2 = m2 .* c2;
s3 = m3 .* c3;
s4 = m4 .* c4;
s5 = m5 .* c5;
s6 = m6 .* c6;

fdm_signal = s1 + s2 + s3 + s4 + s5 + s6;

function y = lowpass(x, n)
    h = ones(1, n)/n;
    y = conv(x, h, "same");
endfunction

r1 = lowpass(fdm_signal .* c1, 100);
r2 = lowpass(fdm_signal .* c2, 100);
r3 = lowpass(fdm_signal .* c3, 100);
r4 = lowpass(fdm_signal .* c4, 100);
r5 = lowpass(fdm_signal .* c5, 100);
r6 = lowpass(fdm_signal .* c6, 100);

figure(0)
subplot(3,1,1); plot(t, m1); title("Message Signal 1 (200Hz)");
subplot(3,1,2); plot(t, m2); title("Message Signal 2 (400Hz)");
subplot(3,1,3); plot(t, m3); title("Message Signal 3 (600Hz)");
figure (1)
subplot(3,1,1); plot(t, m4); title("Message Signal 4 (800Hz)");
subplot(3,1,2); plot(t, m5); title("Message Signal 5 (1000Hz)");
subplot(3,1,3); plot(t, m6); title("Message Signal 6 (1200Hz)");

figure(2)
plot(t, fdm_signal); title("Multiplexed FDM Signal");

figure(3)
subplot(6,1,1); plot(t, r1); title("Demodulated Signal 1");
subplot(6,1,2); plot(t, r2); title("Demodulated Signal 2");
subplot(6,1,3); plot(t, r3); title("Demodulated Signal 3");
subplot(6,1,4); plot(t, r4); title("Demodulated Signal 4");
subplot(6,1,5); plot(t, r5); title("Demodulated Signal 5");
subplot(6,1,6); plot(t, r6); title("Demodulated Signal 6");

disp("✅ FDM and Demodulation completed successfully!");

````
# Output:
<img width="1918" height="1076" alt="image" src="https://github.com/user-attachments/assets/2306b46a-cc10-4633-a0db-edfc30d42e85" />

# Calculation:
![WhatsApp Image 2025-11-28 at 22 46 10_584cf41a](https://github.com/user-attachments/assets/2c10333f-0533-41e3-80d2-8a6374d6ac62)
![WhatsApp Image 2025-11-28 at 22 46 57_a9af032b](https://github.com/user-attachments/assets/fcf2d8d2-f566-4455-af6f-355e967790c6)

# Result:
FDM experiment was successfully simulated and verified using Python. The message signals were multiplexed and perfectly recovered.
