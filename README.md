# Liquor Dispenser Project

## Description

This project is a fully automated liquor dispenser system designed to dispense liquid based on user input. The system uses an Arduino microcontroller to control various components, including a water mini pump, relay module, ultrasonic sensor, and an I2C LCD display. The dispenser allows users to select the volume of liquid, start dispensing, and handle pulses or holds.

## Components

- **Arduino Uno**: Microcontroller to control the system.
- **Water Mini Pump (RS-360S)**: Pumps the liquid from the container.
- **Relay Module (5V)**: Switches the pump on and off.
- **Transistor (TIP120 or MOSFET)**: Optional, for robust motor control.
- **Diode (1N4007)**: Protects the motor circuit from back EMF.
- **12V Power Supply**: External power supply for the water pump.
- **I2C Module for LCD**: Simplifies the connection of the LCD display.
- **Push Buttons**: For selecting volume, starting the pump, and controlling pulse/hold.
- **Resistors (1kΩ or 10kΩ)**: Used with the push buttons as pull-up resistors.
- **Breadboard and Jumper Wires**: For prototyping and connecting components.
- **Ultrasonic Sensor (4-Pin)**: Detects the presence of objects and measures distance.
- **Current-Limiting Resistors**: If needed for the display or buttons.

## Connections

1. **Arduino Uno**: Connect to power source (computer or external).
2. **Water Mini Pump**: Connect to relay module and power supply.
3. **Relay Module**: Connect to Arduino, pump, and power supply.
4. **Transistor (Optional)**: Connect to Arduino and pump (if used).
5. **Diode**: Connect to protect the pump circuit.
6. **12V Power Supply**: Connect to pump.
7. **I2C Module for LCD**: Connect to Arduino.
8. **Push Buttons**: Connect to Arduino with pull-up resistors.
9. **Ultrasonic Sensor**: Connect to Arduino for object detection.
10. **Breadboard and Jumper Wires**: Connect components as needed.

## Code

The code for the project is not final and is subject to changes. It includes:

- Volume selection
- Pump control
- Pulse and hold functionality
- Object detection and automatic cancellation of dispensing

The current code can be found in the `src` directory of this repository. Please review and modify according to your needs.

## Usage

1. **Select Volume**: Press the volume selection button to choose the desired volume.
2. **Start Pump**: Press the start button to begin dispensing the selected volume.
3. **Pulse/Hold**: Use the pulse/hold button for continuous or pulsed dispensing.
4. **Object Detection**: The ultrasonic sensor will ensure an object is detected; otherwise, dispensing will be canceled.

## Contributing

Feel free to contribute to this project by forking the repository and submitting a pull request. All improvements and bug fixes are welcome.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
