# RGB LED Toy

A simple project demonstrating how an RGB LED works. By adjusting potentiometers, users can mix red, green, and blue light to create any color in the RGB spectrum.

![Project Image](assembled-box.jpg)

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Hardware Components](#hardware-components)
- [Enclosure Design](#enclosure-design)
- [Getting Started](#getting-started)
- [Assembly Instructions](#assembly-instructions)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgments](#acknowledgments)

## Overview

This project consists of a PCB board designed in KiCad and an enclosure designed in OpenSCAD, which can be 3D printed. The circuit allows users to adjust the intensity of red, green, and blue channels of an RGB LED using potentiometers, creating a full range of colors.

## Features

- **RGB Color Mixing**: Adjust potentiometers to mix red, green, and blue light.
- **Custom PCB Design**: PCB board designed using KiCad.
- **3D Printable Enclosure**: Box designed in OpenSCAD for housing the PCB and components.

## Hardware Components

- **Electronics**:
  - 1 x Common Cathode RGB LED
  - 3 x Potentiometers (e.g., 10kΩ linear taper)
  - 3 x Current-limiting resistors for the RGB LED (calculate based on your LED specs)
  - 3 x Rocker switches
  - 1 x PCB (design files included)
  - 3 x AAA battery
  - Wires and connectors as needed
  - Some M2 screws
- **Tools Required**:
  - Soldering iron and solder
  - Wire cutters/strippers
  - Multimeter (optional for testing)

Components examples:
- [Red rocker switch](https://www.amazon.com/Twidec-Rocker-Position-Illuminated-KCD2-201N-BU/dp/B07MV5LBX8/ref=sr_1_1?crid=3TD23RVLMG6Z4&dib=eyJ2IjoiMSJ9.MytuyUmSVyDPcqdz0-hPTGBWjusLBUMxMacKpkE82gv4jepQVEn_gehBBpbBIvG3z0AbFAFO2Uk4pilC01WHr0K3-53ic8luo5b2fuP7ekoIxj79-xcMwD2pG09d9zLjWrHabZ0-CDp8GRCul-4eHnmPWNCAGQE1n28UB6E4wumrJ58iK5A6zx9YEQmL51N7-F1vKiq0_7a-CcSb2CmqyDhs0_pPEpygn4H0GRY7VEU.NOZqBpuaGICAkMl2GmvqS3otJYxrt7sDQj98gaYhyTk&dib_tag=se&keywords=Rocker%2BSwitch%2BDPST%2Bblue&qid=1732781497&sprefix=rocker%2Bswitch%2Bdpst%2Bblu%2Caps%2C201&sr=8-1&th=1)
- [Blue rocker switch](https://www.amazon.com/Twidec-Rocker-Position-Illuminated-KCD2-201N-BU/dp/B08YNQX96T/ref=sr_1_1?crid=3TD23RVLMG6Z4&dib=eyJ2IjoiMSJ9.MytuyUmSVyDPcqdz0-hPTGBWjusLBUMxMacKpkE82gv4jepQVEn_gehBBpbBIvG3z0AbFAFO2Uk4pilC01WHr0K3-53ic8luo5b2fuP7ekoIxj79-xcMwD2pG09d9zLjWrHabZ0-CDp8GRCul-4eHnmPWNCAGQE1n28UB6E4wumrJ58iK5A6zx9YEQmL51N7-F1vKiq0_7a-CcSb2CmqyDhs0_pPEpygn4H0GRY7VEU.NOZqBpuaGICAkMl2GmvqS3otJYxrt7sDQj98gaYhyTk&dib_tag=se&keywords=Rocker%2BSwitch%2BDPST%2Bblue&qid=1732781497&sprefix=rocker%2Bswitch%2Bdpst%2Bblu%2Caps%2C201&sr=8-1&th=1)
- [Green rocker switch](https://www.amazon.com/Twidec-Rocker-Position-Illuminated-KCD2-201N-BU/dp/B07MV5TVKX/ref=sr_1_1?crid=3TD23RVLMG6Z4&dib=eyJ2IjoiMSJ9.MytuyUmSVyDPcqdz0-hPTGBWjusLBUMxMacKpkE82gv4jepQVEn_gehBBpbBIvG3z0AbFAFO2Uk4pilC01WHr0K3-53ic8luo5b2fuP7ekoIxj79-xcMwD2pG09d9zLjWrHabZ0-CDp8GRCul-4eHnmPWNCAGQE1n28UB6E4wumrJ58iK5A6zx9YEQmL51N7-F1vKiq0_7a-CcSb2CmqyDhs0_pPEpygn4H0GRY7VEU.NOZqBpuaGICAkMl2GmvqS3otJYxrt7sDQj98gaYhyTk&dib_tag=se&keywords=Rocker%2BSwitch%2BDPST%2Bblue&qid=1732781497&sprefix=rocker%2Bswitch%2Bdpst%2Bblu%2Caps%2C201&sr=8-1&th=1)

## Enclosure Design

The enclosure is designed in OpenSCAD and can be 3D printed. The design files are available in the `openscad_src` directory. Ensure your 3D printer is calibrated correctly for precise fitting of components.

## Getting Started

1. **Clone the Repository**: Clone this repository to your local machine using `git clone https://github.com/KubaTaba1uga/electronics_rgb_led_toy.git`.
2. **Review the Schematics**: Open the KiCad files located in the `kicad_src` directory to understand the circuit design.
3. **Prepare the PCB**: Manufacture the PCB using the provided design files or use a prototyping board for initial testing.
4. **Gather Components**: Ensure all necessary components are available before assembly.

## Assembly Instructions

1. **Solder Components**: Solder the RGB LED, potentiometers, resistors, and connectors onto the PCB as per the schematic.
2. **Assemble Enclosure**: 3D print the enclosure and place the assembled PCB inside, securing it appropriately.

## Usage

- **Adjust Colors**: Rotate the potentiometers to vary the intensity of each color channel (red, green, blue) and observe the resulting color from the RGB LED.
- **Experiment**: Mix different levels to create various colors and understand additive color mixing principles.

## Contributing

Contributions are welcome! Please fork the repository and create a pull request with your proposed changes. Ensure your code adheres to the project's coding standards and includes appropriate documentation.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

## Acknowledgments

- Inspired by basic electronics and the desire to make learning interactive.
- Thanks to the communities of [KiCad](https://www.kicad.org/) and [OpenSCAD](https://www.openscad.org/) for their amazing tools.
- Thanks to Charles platt (Make: Electronics) and Paul Sherz, Simon Monk (Practical Electronics for Inventors) for their awesome books.
