// Oscilloscope using Arduino with Moving Average Filter and Voltage Calculation

// Define the analog input pin
const int analogInPin = A0; 

// Define the sampling rate (in milliseconds)
const int sampleInterval = 1; // Adjust this for faster/slower sampling

// Define the number of samples for averaging
const int numSamples = 130;

// Define the reference voltage (change to 3.3 if using 3.3V reference)
const float referenceVoltage = 5.0;

// Moving average buffer
int readings[numSamples];      // the readings from the analog input
int readIndex = 0;             // the index of the current reading
long total = 0;                // the running total

// Threshold to consider input as zero
const int threshold = 10;

void setup() {
  // Initialize all the readings to 0
  for (int thisReading = 0; thisReading < numSamples; thisReading++) {
    readings[thisReading] = 0;
  }
  
  // Start the serial communication at 115200 baud rate
  Serial.begin(115200);
}

void loop() {
  // Subtract the last reading
  total = total - readings[readIndex];
  
  // Read the analog input
  int sensorValue = analogRead(analogInPin);

  // Apply threshold
  if (sensorValue < threshold) {
    sensorValue = 0;
  }

  // Store the new reading
  readings[readIndex] = sensorValue;
  
  // Add the reading to the total
  total = total + readings[readIndex];
  
  // Advance to the next position in the array
  readIndex = readIndex + 1;
  
  // If we're at the end of the array, wrap around to the beginning
  if (readIndex >= numSamples) {
    readIndex = 0;
  }
  
  // Calculate the average
  float average = total / numSamples;
  
  // Convert the average value to voltage
  float voltage = (average / 1023.0) * referenceVoltage;
  
  // Send the voltage value over the serial port
  Serial.println(voltage, 3); // Print voltage with 3 decimal places
  
  // Wait for the next sample
  delay(sampleInterval);
}
