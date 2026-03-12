+++
template = "post.html"

title = "`tosca` is born!"
description = """
`tosca` is a new IoT framework that enables developers to design firmware for
several hardware architectures, along with the corresponding controllers
to interact with them. Its security-by-design approach helps reduce compile-time
bugs, while its hazard system exposes the safety, economic, and privacy risks of
device commands.
"""
date = 2026-02-05
tags = ["rust", "secure-by-design", "tosca"]

[extra.languages]
message = "This post can also be read in "
translations = [
  { lang = "it", path = "/it/blog/nascita-di-tosca", name = "Italian" }
]
+++

The `tosca` framework has finally come to light! 🎉🎉

Started as an academic project aimed at designing an IoT architecture that makes
transparent to a user the risks of executing device commands, over the past two
years `tosca` has evolved into a **customizable and self-contained framework**.

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

# Main Features

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

# Still Missing Features

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

# `tosca` Structure

The framework revolves around the `tosca` interface, which acts as a bridge
between the two sides of the framework.
The first side is the **Firmware Side**, which provides APIs for developing
firmware and drivers for sensors.
The second side is the **Controller Side**, which provides APIs for interacting
with devices built using the `tosca` framework.

The [`tosca`](https://github.com/ToscaLabs/tosca/tree/master/crates/tosca)
interface is the core component of the framework. It is used by both the
firmware side and controller side APIs to provide a unified set of definitions.

It can:

* **Create and manage REST routes** that issue commands from a controller to a
device. Each route can define parameters that mirror those used by the
corresponding device command. Route responses are of various types:
  * `Ok`: indicates that the command was successfully executed on the device.
  * `Serial`: returns additional data related to the device operation.
  * `Info`: provides metadata and other details about the device.
  * `Stream` (optional feature): delivers chunks of multimedia data as
  byte streams.
* **Provide a description of a device**, including its internal data,
available methods, and information about its economic and energy consumption.
* **Associate hazards with a route.** As mentioned earlier, hazards
are categorized into three types: *Safety*, *Financial*, and *Privacy*.
  * *Safety* hazards refer to potential risks to human life.
  * *Financial* hazards concern the economic impact of the device and
  its commands.
  * *Privacy* hazards relate to issues involving the handling and management of
  sensitive data.

## Firmware Side

The firmware side provides the APIs required to develop a `tosca`-compliant
firmware device. These APIs are split into two library crates according to the
target hardware architecture:

* [`tosca-os`](https://github.com/ToscaLabs/tosca/tree/master/crates/tosca-os)
* [`tosca-esp32c3`](https://github.com/ToscaLabs/tosca/tree/master/crates/tosca-esp32c3)

The APIs are largely equivalent across the two crates, and both integrate the
`tosca` crate to maintain a common interface between firmware and controller
components.

The [`tosca-os`](https://github.com/ToscaLabs/tosca/tree/master/crates/tosca-os)
library crate is designed for firmware running on operating systems. Using these
APIs, we have been able to implement firmware for devices such as a smart light
and an IP camera.

The
[`tosca-esp32c3`](https://github.com/ToscaLabs/tosca/tree/master/crates/tosca-esp32c3)
library crate is instead designed for building firmware that runs on `ESP32-C3`
microcontrollers.

It provides APIs to:

* Connect a device to a `Wi-Fi` access point
* Build and configure the network stack
* Configure the `mDNS-SD` discovery service
* Define events for specific route tasks
* Initialize and run an `HTTP` server

The device APIs are designed to guide developers in defining their own devices,
minimizing ambiguities that could arise during firmware development.

To demonstrate the capabilities of these APIs, we implemented several
**light firmware device examples** with increasing levels of complexity.

Finally, we also created an additional library crate that provides
architecture-agnostic drivers for sensors that are easy to integrate but often
lack dedicated drivers:
[`tosca-drivers`](https://github.com/ToscaLabs/tosca/tree/master/crates/tosca-drivers).
All drivers are built on top of existing crates that ensure compatibility across
the supported hardware platforms.

## Controller Side

The controller side provides the APIs required to build software capable of
interacting with all `tosca`-compliant firmware devices within the same network.
To use these APIs, a controller application simply needs to include the
[`tosca-controller`](https://github.com/ToscaLabs/tosca/tree/master/crates/tosca-controller)
library crate as a dependency.

The core functionalities of this crate include:

* **Discovering devices**: automatically discovering all `tosca`-compliant
firmware devices available on the network.
* **Issuing device commands**: using a device’s description to construct the
corresponding *REST* requests and send them to invoke its commands.
* **Enforcing security and privacy policies**: defining policies that allow or
block requests that do not comply with the specified rules.
* **Subscribing to device events**: intercepting events published by devices
that represent their internal state, by subscribing to the device brokers
provided in their descriptions.

# Conclusions
