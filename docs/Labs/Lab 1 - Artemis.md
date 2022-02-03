---
title: Lab 1 - Artemis Board
---
# Author: Swapnil Barot (NetID: spb228)
---

[Return to Main Page](https://spbarot.github.io/)

## I. Objective

The goal of this lab is to configure the Sparkfun RedBoard Artemis Nano Board (microcontroller) and the Arduino IDE. Upon verifying the system functionality, the experimenters shall compile and upload example code (with modifications) such as Blink It Up, Serial, AnalogRead, and MicrophoneOutput. Lastly, ECE 5960 students shall develop a program to enable the on-board LED when the microphone senses a whistle. Refer to the Sparkfun RedBoard Artemis Nano datasheet and Artemis setup instructions in the appendix section for preliminary information required for the lab. 

---
## II. Materials/Software

1. 1x SparkFun RedBoard Artemis Nano
2. 1x USB A to C Cable
3. Arduino IDE (Software)

---
## III. Procedure/Design/Results

#### System Setup

First download and install the Arduino IDE. Then, Interconnect the Artemis and the computer via the USB cable. Once the program is installed, hover to Tools and select Boards Manager. Download and install the Sparkfun Apollo 3 Boards package. 

---

#### Blink It Up Example
  
  Once the system is setup, select File, Examples, 01. Basics and open Blink. Analyze the software and select Upload on the toolbar. 
The Blink program enables the on-board blue LED. As per the algorithm, the LED is toggled on and off every 1000 milliseconds. Note that the LED on/off frequency can be modified by adjusting the delay. The video showcases a delay of 100 milliseconds, increasing the on/off frequency. The on-board LED is an extremely useful tool when debugging and verifying software functionalities. 

  <iframe width="560" height="315" src="https://www.youtube.com/embed/FCxQfeuSuMI" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  
---

#### Serial Example
  
  Select File, Examples, Apollo 3, and open Example04_Serial. 
The serial program verifies the functionality of the serial communication, serial monitor and serial port. The program prints out an incrementing print statement and prompts the user to enter data which echoes in the serial monitor. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/QihvJWqAIBk" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

#### AnalogRead Example
  
  Select File, Examples, Apollo 3, and open Example02_AnalogRead.
AnalogRead takes advantage of the on-board ADC (analog to digital converter) and can read analog voltages from 0V to 2V. The ADC channel measures the internal die temperature, VCC voltage, and VSS voltage. Additionally, the program is designed to fade the on-board LED to match the voltage reading on the respective analog pin. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/N4dWy57jVsg" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

#### MicrophoneOutput Example
  
  Select File, Examples, PDM, and open Example1_MicrophoneOutput.
MicrophoneOutput is designed to enable the PDM microphone to continuously read input and output the loudest frequency detected. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZEnoK6dK2mE" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

#### Microphone_LED
  
  Microphone_LED is a program that enables the on-board LED when the microphone senses a whistle. To carryout this design intent, an if/else statement is utilized that turns the LED on if a frequency of > 4000 Hz is detected and turns the LED off if the recorded frequency is <=4000 Hz. Refer to the appendix section for the code utilized in Microphone_LED. 
  
  <iframe width="560" height="315" src="https://www.youtube.com/embed/J9VLsoFVJos" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  
---

## IV. Conclusion

The experimenters successfully configured the Artemis board and the Arduino IDE while running several example programs to verify the system functionality. Knowledge gained in this lab with respect to system setup, microcontroller familiarity, on-board peripherals, and programming will assist the experiments in future labs. Most of the experiment was quite smooth, except for AnalogRead, which contained bugs that did not allow the software to present the computed/processed data. Instead, the program prints raw data in the form of bits. The lab writeup and the given references proved to be helpful throughout the entire lab. 

---

## V. Code Appendix

#### Microphone_LED Code


```

/* Author: Nathan Seidle
  Editor: Swapnil Barot (spb228)
  Created: July 24, 2019
  Edited_v1: February 2, 2022

  This example demonstrates how to use the pulse density microphone (PDM) on Artemis boards.
  This library and example are heavily based on the Apollo3 pdm_fft example.
*/

/* 
// This file is subject to the terms and conditions defined in
// file 'LICENSE.md', which is part of this source code package.
*/

//Global variables needed for PDM library
#define pdmDataBufferSize 4096 //Default is array of 4096 * 32bit
uint16_t pdmDataBuffer[pdmDataBufferSize];

//Global variables needed for the FFT in this sketch
float g_fPDMTimeDomain[pdmDataBufferSize * 2];
float g_fPDMFrequencyDomain[pdmDataBufferSize * 2];
float g_fPDMMagnitudes[pdmDataBufferSize * 2];
uint32_t sampleFreq;

//Enable these defines for additional debug printing
#define PRINT_PDM_DATA 0
#define PRINT_FFT_DATA 0

#include <PDM.h> //Include PDM library included with the Aruino_Apollo3 core
AP3_PDM myPDM;   //Create instance of PDM class

//Math library needed for FFT
#include <arm_math.h>

void setup()
{

  pinMode(LED_BUILTIN, OUTPUT);
  Serial.begin(115200);
  Serial.println("SparkFun PDM Example");

  if (myPDM.begin() == false) // Turn on PDM with default settings, start interrupts
  {
    Serial.println("PDM Init failed. Are you sure these pins are PDM capable?");
    while (1)
      ;
  }
  Serial.println("PDM Initialized");

  printPDMConfig();
}

void loop()
{

  

  //digitalWrite(LED_BUILTIN, LOW); 
  //delay(1000);
  
  if (myPDM.available())
  {
    myPDM.getData(pdmDataBuffer, pdmDataBufferSize);

    printLoudest();
  }

  // Go to Deep Sleep until the PDM ISR or other ISR wakes us.
  am_hal_sysctrl_sleep(AM_HAL_SYSCTRL_SLEEP_DEEP);
}

//*****************************************************************************
//
// Analyze and print frequency data.
//
//*****************************************************************************
void printLoudest(void)
{
  float fMaxValue;
  uint32_t ui32MaxIndex;
  int16_t *pi16PDMData = (int16_t *)pdmDataBuffer;
  uint32_t ui32LoudestFrequency;

  //
  // Convert the PDM samples to floats, and arrange them in the format
  // required by the FFT function.
  //
  for (uint32_t i = 0; i < pdmDataBufferSize; i++)
  {
    if (PRINT_PDM_DATA)
    {
      Serial.printf("%d\n", pi16PDMData[i]);
    }

    g_fPDMTimeDomain[2 * i] = pi16PDMData[i] / 1.0;
    g_fPDMTimeDomain[2 * i + 1] = 0.0;
  }

  if (PRINT_PDM_DATA)
  {
    Serial.printf("END\n");
  }

  //
  // Perform the FFT.
  //
  arm_cfft_radix4_instance_f32 S;
  arm_cfft_radix4_init_f32(&S, pdmDataBufferSize, 0, 1);
  arm_cfft_radix4_f32(&S, g_fPDMTimeDomain);
  arm_cmplx_mag_f32(g_fPDMTimeDomain, g_fPDMMagnitudes, pdmDataBufferSize);

  if (PRINT_FFT_DATA)
  {
    for (uint32_t i = 0; i < pdmDataBufferSize / 2; i++)
    {
      Serial.printf("%f\n", g_fPDMMagnitudes[i]);
    }

    Serial.printf("END\n");
  }

  //
  // Find the frequency bin with the largest magnitude.
  //
  arm_max_f32(g_fPDMMagnitudes, pdmDataBufferSize / 2, &fMaxValue, &ui32MaxIndex);

  ui32LoudestFrequency = (sampleFreq * ui32MaxIndex) / pdmDataBufferSize;

    if (ui32LoudestFrequency > 4000)
      {
       digitalWrite(LED_BUILTIN, HIGH);   
       Serial.printf("Whistle Detected\n"); 
      }

    else
      {
       digitalWrite(LED_BUILTIN, LOW);
       Serial.printf("No Whistle Detected\n");  
      }

  if (PRINT_FFT_DATA)
  {
    Serial.printf("Loudest frequency bin: %d\n", ui32MaxIndex);
  }

  Serial.printf("Loudest frequency: %d         \n", ui32LoudestFrequency);
}

//*****************************************************************************
//
// Print PDM configuration data.
//
//*****************************************************************************
void printPDMConfig(void)
{
  uint32_t PDMClk;
  uint32_t MClkDiv;
  float frequencyUnits;

  //
  // Read the config structure to figure out what our internal clock is set
  // to.
  //
  switch (myPDM.getClockDivider())
  {
  case AM_HAL_PDM_MCLKDIV_4:
    MClkDiv = 4;
    break;
  case AM_HAL_PDM_MCLKDIV_3:
    MClkDiv = 3;
    break;
  case AM_HAL_PDM_MCLKDIV_2:
    MClkDiv = 2;
    break;
  case AM_HAL_PDM_MCLKDIV_1:
    MClkDiv = 1;
    break;

  default:
    MClkDiv = 0;
  }

  switch (myPDM.getClockSpeed())
  {
  case AM_HAL_PDM_CLK_12MHZ:
    PDMClk = 12000000;
    break;
  case AM_HAL_PDM_CLK_6MHZ:
    PDMClk = 6000000;
    break;
  case AM_HAL_PDM_CLK_3MHZ:
    PDMClk = 3000000;
    break;
  case AM_HAL_PDM_CLK_1_5MHZ:
    PDMClk = 1500000;
    break;
  case AM_HAL_PDM_CLK_750KHZ:
    PDMClk = 750000;
    break;
  case AM_HAL_PDM_CLK_375KHZ:
    PDMClk = 375000;
    break;
  case AM_HAL_PDM_CLK_187KHZ:
    PDMClk = 187000;
    break;

  default:
    PDMClk = 0;
  }

  //
  // Record the effective sample frequency. We'll need it later to print the
  // loudest frequency from the sample.
  //
  
  sampleFreq = (PDMClk / (MClkDiv * 2 * myPDM.getDecimationRate()));

  frequencyUnits = (float)sampleFreq / (float)pdmDataBufferSize;

  Serial.printf("Settings:\n");
  Serial.printf("PDM Clock (Hz):         %12d\n", PDMClk);
  Serial.printf("Decimation Rate:        %12d\n", myPDM.getDecimationRate());
  Serial.printf("Effective Sample Freq.: %12d\n", sampleFreq);
  Serial.printf("FFT Length:             %12d\n\n", pdmDataBufferSize);
  Serial.printf("FFT Resolution: %15.3f Hz\n", frequencyUnits);  
}

```

---


## VI. References

1. [SparkFun RedBoard Artemis Nano]( https://www.sparkfun.com/products/15443)

2. [Setup Instructions]( https://learn.sparkfun.com/tutorials/artemis-development-with-arduino?_ga=2.30055167.1151850962.1594648676-1889762036.1574524297&_gac=1.19903818.1593457111.Cj0KCQjwoub3BRC6ARIsABGhnyahkG7hU2v-0bSiAeprvZ7c9v0XEKYdVHIIi_-J-m5YLdDBMc2P_goaAtA4EALw_wcB)

---

[Return to Main Page](https://spbarot.github.io/)



