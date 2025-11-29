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
t = linspace(0, 1, 1000);
fs = 1000;

freqs = [6 11 17 23 29 36];
amps = [1 2 3 4 5 6];

signals = zeros(6, length(t));
for i = 1:6
    signals(i,:) = amps(i) * sin(2*%pi*freqs(i)*t);
end

fdm_signal = zeros(1, length(t));
for i = 1:6
    fdm_signal = fdm_signal + signals(i,:);
end

order = 50;
cutoff_freq = 10 / (fs/2);
h = ffilt("lp", order, cutoff_freq);

demux = zeros(6, length(t));
for i = 1:6
    mixed = fdm_signal .* sin(2*%pi*freqs(i)*t);
    demux(i,:) = filter(h, 1, mixed);
end

scf(1);
clf;
for i = 1:6
    subplot(3,2,i);
    plot(t,signals(i,:));
    title("Original Signal f=" + string(freqs(i)));
end

scf(2);
clf;
plot(t,fdm_signal);
title("FDM Signal");

scf(3);
clf;
for i = 1:6
    subplot(3,2,i);
    plot(t,demux(i,:));
    title("Demultiplexed Signal f=" + string(freqs(i)));
end



````
# Output:
<img width="1257" height="712" alt="image" src="https://github.com/user-attachments/assets/ab27700d-7e92-45b2-b368-7ed0c6647b2a" />
<img width="676" height="528" alt="image" src="https://github.com/user-attachments/assets/d405ae88-0f10-47f3-aedc-fc06b2db06c3" />
<img width="1186" height="673" alt="image" src="https://github.com/user-attachments/assets/6921f8d0-685e-4792-b8ee-e284981646af" />



# Calculation:
![WhatsApp Image 2025-11-29 at 15 46 05_2a150197](https://github.com/user-attachments/assets/673d2c8d-194c-4495-9a2c-8cfc8256d029)

![WhatsApp Image 2025-11-29 at 15 46 13_34536e63](https://github.com/user-attachments/assets/867fa8ce-9067-45c5-8fdf-17b292a945d1)


# Result:
FDM experiment was successfully simulated and verified using Python. The message signals were multiplexed and perfectly recovered.
