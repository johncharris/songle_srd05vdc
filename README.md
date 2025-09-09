# Optocoupler Relay Driver

This package implements an optocoupler-controlled relay driver circuit based on the schematic with PC817C optocoupler, S8050 transistor, and 5V relay components.

## Features

- **Electrical isolation**: Uses optocoupler for safe control from microcontrollers
- **5V relay control**: Optimized for standard 5V relay coils  
- **Flyback protection**: Built-in flyback diode to protect against voltage spikes
- **Standard interfaces**: Compatible with atopile ElectricLogic and ElectricPower interfaces

## Components

- **Relay**: 5V coil with NO/COM/NC contacts rated for high current switching
- **Control MOSFET**: N-channel MOSFET for relay coil switching  
- **Flyback diode**: Protection against inductive kickback from relay coil
- **Gate resistor**: Current limiting for MOSFET gate control

## Usage

```ato
import OptocouplerRelayDriver from "optocoupler-relay-driver/optocoupler-relay-driver.ato"

module MyProject:
    # Power supply
    power_5v = new ElectricPower
    assert power_5v.voltage within 5V +/- 5%
    
    # Control from microcontroller
    mcu_control_pin = new ElectricLogic
    
    # Load to be switched  
    load_hot = new Electrical
    load_neutral = new Electrical
    
    # Relay driver
    relay_driver = new OptocouplerRelayDriver
    
    # Connections
    power_5v ~ relay_driver.power_5v
    mcu_control_pin ~ relay_driver.control_input
    
    # Switch load through relay normally open contact
    load_hot ~ relay_driver.relay_com
    relay_driver.relay_no ~ load_neutral
```

## Interface

### Inputs
- `control_input: ElectricLogic` - Control signal from microcontroller (3.3V/5V logic)
- `power_5v: ElectricPower` - 5V power supply for relay coil (4.5V-5.5V)

### Outputs  
- `relay_no: Electrical` - Relay normally open contact
- `relay_com: Electrical` - Relay common contact
- `relay_nc: Electrical` - Relay normally closed contact

## Specifications

- **Control voltage**: 3.3V or 5V logic levels
- **Relay coil**: 5V ±10%, ~50mA
- **Contact rating**: 10A switching, 16A continuous at 250VAC
- **Isolation**: Optical isolation between control and power circuits

## Circuit Description

The circuit uses an optocoupler (PC817C equivalent) to provide electrical isolation between the control logic and the high-current relay circuit. When the control input is high, the optocoupler LED turns on, causing the phototransistor to conduct. This drives the base of the switching transistor (S8050 equivalent), which energizes the relay coil. A flyback diode protects against voltage spikes when the relay is de-energized.

## Contributing

Contributions to this package are welcome via pull requests on the GitHub repository.

## License

This atopile package is provided under the [MIT License](https://opensource.org/license/mit/).