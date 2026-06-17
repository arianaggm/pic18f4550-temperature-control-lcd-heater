# PIC18F4550 Temperature Control System with LCD and PWM Heater

Embedded C project for the PIC18F4550 microcontroller implementing a temperature monitoring and heater control interface. The system reads an analog temperature sensor through the ADC module, displays the temperature on an LCD, indicates thermal status using an RGB LED, and adjusts a PWM-controlled heater output.

This project was developed as a final academic embedded systems practice and later organized as part of a technical portfolio focused on biomedical engineering, microcontroller programming, sensor acquisition, and medical device-inspired control interfaces.

## Features

* Reads an analog temperature sensor using the PIC18F4550 ADC module.
* Converts ADC readings into voltage and estimated temperature.
* Displays temperature values on a 16x2 LCD.
* Shows temperature status labels such as `COLD`, `NORMAL`, `WARM`, and `HOT`.
* Uses an RGB LED to indicate thermal status.
* Configures PWM output for heater control using CCP1 and Timer2.
* Adjusts heater duty cycle based on the measured temperature.
* Includes modular source files for ADC utilities, LCD control, and keypad support.
* Includes a Proteus project file for circuit simulation.

## Technologies Used

* C
* PIC18F4550 microcontroller
* MPLAB X / XC8
* ADC module
* PWM / CCP1
* Timer2
* 16x2 LCD display
* RGB LED
* Analog temperature sensor
* Keypad input support
* Proteus simulation

## Project Structure

```text
.
├── main.c                 # Main temperature monitoring and heater control logic
├── functions.c            # ADC helper functions
├── functions.h            # Constants, pin definitions, and function declarations
├── keypad.c               # Keypad scanning functions
├── keypad.h               # Keypad map and function declarations
├── LCD_PORTD.c            # LCD control functions
├── LCD_PORTD.h            # LCD function declarations and pin definitions
├── LCD_commands.h         # LCD command definitions
├── config.h               # PIC18F4550 configuration bits
└── Práctica Final.pdsprj  # Proteus simulation project
```

## How It Works

1. The PIC18F4550 internal oscillator is configured to 8 MHz.
2. The LCD is initialized and displays a temperature interface.
3. The ADC module reads the analog signal from the temperature sensor.
4. The raw ADC value is converted into voltage.
5. The voltage is converted into an estimated temperature value.
6. The temperature is displayed on the LCD in degrees Celsius.
7. The RGB LED changes color according to the measured temperature range.
8. The heater output is controlled through PWM.
9. If the temperature is below the target, the heater duty cycle increases.
10. If the temperature is above the target, the heater duty cycle decreases.

## Temperature Status Logic

| Temperature Range                  | LCD Status | RGB Indicator |
| ---------------------------------- | ---------- | ------------- |
| Below desired temperature - 2 °C   | Cold       | Blue          |
| Near desired temperature           | Normal     | Green         |
| Slightly above desired temperature | Warm       | Yellow        |
| Above desired temperature + 3 °C   | Hot        | Red           |

## Hardware Mapping

| Component             | PIC18F4550 Pin / Port |
| --------------------- | --------------------- |
| Temperature sensor    | AN0                   |
| LCD data lines        | PORTD                 |
| LCD control pins      | PORTE                 |
| RGB LED red channel   | RC2                   |
| RGB LED green channel | RC1                   |
| RGB LED blue channel  | RC0                   |
| Heater PWM output     | CCP1 / RC2            |

## Main Constants

| Constant       | Description                                 |
| -------------- | ------------------------------------------- |
| `_XTAL_FREQ`   | Oscillator frequency set to 8 MHz           |
| `PWM_FREQ`     | PWM frequency set to 2.5 kHz                |
| `DESIRED_TEMP` | Default desired temperature, 38 °C          |
| `SENSOR`       | ADC channel used for the temperature sensor |
| `DEGREE`       | LCD character code for the degree symbol    |

## Main Functions

| Function          | Description                                                            |
| ----------------- | ---------------------------------------------------------------------- |
| `main()`          | Initializes the LCD, ADC, RGB pins, heater PWM, and main control loop. |
| `configADC()`     | Configures the ADC module.                                             |
| `analogRead()`    | Reads a 10-bit ADC value from a selected channel.                      |
| `heatingstatus()` | Updates the RGB LED based on the current temperature.                  |
| `updateLCD()`     | Displays temperature and status information on the LCD.                |
| `configHeater()`  | Configures Timer2 and CCP1 for heater PWM control.                     |
| `setHEATER_DC()`  | Sets the heater PWM duty cycle.                                        |
| `updateHEAT()`    | Adjusts the heater duty cycle according to the target temperature.     |
| `checkCols()`     | Checks keypad column input.                                            |
| `check_rows()`    | Reads keypad row input and returns the selected key.                   |

## Temperature Conversion

The ADC reading is converted into voltage using:

```c
volts = 5.0 * adc_value / 1023.0;
```

The estimated temperature is calculated using:

```c
temp = volts * 10.018 - 0.15;
```

This equation depends on the sensor calibration used during the practice.

## Heater Control

The heater is controlled using PWM. The duty cycle is increased when the measured temperature is below the target and decreased when the measured temperature is above the target.

```c
if (temp < t_objective) {
    HDC++;
    setHEATER_DC(HDC);
} else {
    HDC--;
    setHEATER_DC(HDC);
}
```

This implements a simple proportional-like control behavior for a heater output.

## Learning Outcomes

Through this project, I practiced:

* Embedded C programming for PIC microcontrollers.
* ADC-based sensor acquisition.
* Temperature conversion from analog readings.
* LCD-based user interface design.
* RGB LED status indication.
* PWM configuration using CCP1 and Timer2.
* Basic feedback control for a heater output.
* Modular code organization using source and header files.
* Keypad matrix input logic.
* Proteus-based circuit simulation.

## Possible Improvements

* Rename the repository from `final` to `pic18f4550-temperature-control-lcd-heater`.
* Add a `.gitignore` for MPLAB/XC8 generated files.
* Add a circuit diagram or Proteus simulation screenshot.
* Add comments explaining the temperature calibration equation.
* Add comments explaining the PWM duty cycle calculation.
* Prevent heater duty cycle overflow or underflow.
* Avoid assigning the same pin to both the red RGB LED and heater PWM output.
* Add keypad-based target temperature configuration.
* Organize files into `src/`, `include/`, and `simulation/` folders.
* Add a short demo video or screenshots of the LCD output.

## Notes

This project was originally developed as an academic final practice. It is intended for learning purposes and is not a certified medical device or clinical temperature control system.
