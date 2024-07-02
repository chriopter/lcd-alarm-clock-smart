# lcd-alarm-clock-smart

## Overview
Connect a normal LCD alarm so you can use HA to control blinds, lights, and radio upon alarm activation. Use deep sleep to allow battery operation.

## Hardware

- Alarm Clock with Beeper (in this case Lexon Flip with DCF77 RTC (i think)
- ESP32-C3 Microcontroller
- Low Pass Filter, in my case	Resistor with 10 Ω &	Capacitor (0.1 µF)

## Wiring the Low Pass Filter
The low pass filter is to smooth the signal from the beeper for correct detection.
-	Connect Beeper -> Resistor -> Capacitator -> GPIO0

## Wiring Wakeup Pin
In my case, the low pass filter prevented wakeup so i pulled a second wire. Also for this clock, the beeper always receives voltage so wakeup pin must inverted to trigger in ESPHome code.
- Connect Beeper -> GPIO3

## Flash ESP
[Use this code](Wecker.yaml)
- ESP is in Deep Sleep
- Alarm wakes it up
- Waits on definitive Alarm signal to trigger a sensor
- Deep Sleep after no alarm for 5s or 60s in any case

## Setup

1.	Set alarm time as usual.
2.	Configure HomeAssistant to recognize the Sensor and execute actions.
3.	Power Management: ESP32 will sleep post-alarm, waking only for beeper signals.

## Energy Consumption
With two AAA batteries (totaling approx. 3,000 mAh at 3.0V), the setup should last over a year on a single charge for the ESP alone.
- Active Mode: Consumes maybe 60mA-100mA; typically operational for 60 seconds daily.
- Deep Sleep Mode: Draws 5µA; active most of the day.
