# lcd-alarm-clock-smart

## Overview
Connect a normal LCD alarm so you can use HA to control blinds, lights, and radio upon alarm activation. Use deep sleep to allow battery operation on 2xAAA of the alarm clock.

# Installation

## Hardware

- Alarm Clock with Beeper (in this case Lexon Flip with DCF77 RTC (i think)
- ESP32-C3 Microcontroller
- Low Pass Filter, in my case	Resistor with 10 Ω &	Capacitor (0.1 µF)

## Wiring Power
- Wire Up 3.3V and GND Pins of the ESP to the Battery. In my testing, the ESP32 even runs on 2xAAA Rechargeable Eneloops (2,4v).
- If unstable, use DC to DC converter to bring input voltage to 3,3/5v.

## Wiring the Low Pass Filter
The low pass filter is to smooth the signal from the beeper for correct detection.
-	Connect Beeper -> Resistor -> Capacitator -> GPIO0

## Wiring Wakeup Pin
In my case, the low pass filter prevented wakeup so i pulled a second wire. Also for this clock, the beeper always receives voltage so wakeup pin must inverted to trigger in ESPHome code.
- Connect Beeper -> GPIO3
<img width="250" alt="beeper wire" src="https://github.com/chriopter/lcd-alarm-clock-smart/assets/82179548/70159995-f6fe-46b5-af25-0b57c11fdf90">
<img width="250" alt="wired" src="https://github.com/chriopter/lcd-alarm-clock-smart/assets/82179548/88678080-b19e-435f-904c-c24c5a8993ec">
<img width="250" alt="installed" src="https://github.com/chriopter/lcd-alarm-clock-smart/assets/82179548/ed5c5c60-8dab-4e11-a2a2-7f17d929e765">
<img width="250" alt="finished" src="https://github.com/chriopter/lcd-alarm-clock-smart/assets/82179548/60b2974d-0bf7-4be9-b1dd-fa4bd612b2dd">



# Usage

## Flash ESP
[Use this code](Wecker.yaml)
- ESP is in Deep Sleep
- Alarm wakes it up
- Waits on definitive Alarm signal to trigger a sensor
- Reflects snooze with state reset
- Deep Sleep after 60s (in the future could be kept awake while beeping)
<img width="250" alt="signal" src="https://github.com/chriopter/lcd-alarm-clock-smart/assets/82179548/623985fa-9a9c-419d-8a0f-7bb44fc31f5c">

## Setup

1.	Set alarm time as usual.
2.	Configure HomeAssistant to recognize the Sensor and execute actions.
3.	Power Management: ESP32 will sleep post-alarm, waking only for beeper signals. Just re-insert batteries if you want to trigger an ESP Boot.

## Energy Consumption
With two AAA batteries (totaling approx. 3,000 mAh at 3.0V), the setup should last over a year on a single charge for the ESP alone.
- Active Mode: Consumes maybe 60mA-100mA; typically operational for 60 seconds daily.
- Deep Sleep Mode: Draws 5µA; active most of the day.
