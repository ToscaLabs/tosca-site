+++
template = "post.html"

title = "tosca: a customizable and self-contained IoT framework"
description = """
`tosca` enables developers to design firmware for multiple hardware
architectures and controllers to interact with them.
Its security-by-design approach reduces compile-time bugs, while its hazards
expose the safety, economic, and privacy risks of device operations.
"""
date = 2026-02-05
tags = ["rust", "secure-by-design", "tosca"]

[extra.languages]
message = "This post can also be read in "
translations = [
  { lang = "it", path = "/it/blog/nascita-di-tosca", name = "Italian" }
]
+++

The `tosca` framework has finally come to light! 🎉

Started as an academic project to design an IoT architecture that exposes the
risks of executing device commands, over the past two years `tosca` has evolved
into a **customizable and self-contained framework**.

By *customizable*, we mean that firmware developers can expose some or all
device commands to external interfaces using `tosca` APIs, without significantly
changing the internal firmware structure.
These APIs assign a route to each command and, during firmware startup,
aggregate all route information into a single file that serves as the firmware
description.
External software can discover the device on the network using the `tosca`
controller APIs and then make a server request to obtain its description file.
By parsing this file, the APIs provide the ability to control the device
commands and configure their parameters.

By *self-contained*, we mean that the framework does not rely on any external
dependencies to establish communication between a firmware and its controller.

## Main Features

* **Designed for IoT applications**, from _home automation_ to _industrial
processes_, including the development of devices for social purposes, such as
supporting people with disabilities and older adults.
* **Modular Firmware APIs** for building firmware components such as firmware
descriptions, discovery protocols, and server definitions.
* **Customizable Controller APIs** for discovering devices on a network,
executing their commands, and managing their privacy information.
* **Command Hazards** are labels attached to device commands to notify users
of potential _safety_, _privacy_, and _financial_ hazards during execution.
* **Safe programming with Rust** to guarantee memory and thread safety,
preventing many classes of bugs at compile time.

## Still Missing Features

- **Secure communication** between firmware devices and controllers, ensuring
confidentiality, integrity, and authentication.
- **Over-the-Air (OTA)** firmware updates, enabling remote and reliable device
upgrades.
- **Bluetooth stack integration** for wireless connectivity and device
communication.
- **Accurate energy and performance metrics**, allowing precise on-device
monitoring and analysis.
- **Support for additional microcontroller architectures**, expanding hardware
compatibility and portability.
- **Command scheduling system**, enabling controllers to execute device commands
at specific times or according to defined schedules.
- **Reduced heap allocations on firmware devices**, improving memory efficiency,
predictability, and suitability for resource-constrained environments.

# APIs showcase

# Conclusion
