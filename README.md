Overview
This Arduino code is designed to read an analog signal from a pin, calculate a smoothed voltage using a moving average filter, and display the result over the serial monitor.
It can be used to monitor slow-changing or low-frequency analog signals while applying a noise-reducing filter to improve accuracy.

Features
Voltage Measurement: Reads analog voltages between 0V and the reference voltage (5V or 3.3V, depending on the board).
Moving Average Filter: Smoothens the signal by averaging a series of samples (default: 130 samples).
Thresholding: Ignores readings below a defined threshold to minimize noise interference (default: 10 ADC units).
Real-Time Monitoring: Outputs the smoothed voltage to the serial monitor at a user-defined sampling rate (default: 1 ms).
