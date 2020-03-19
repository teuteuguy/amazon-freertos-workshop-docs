---
title: Supported devices
layout: single
sidebar:
- nav: docs
permalink: /docs/supported-devices
excerpt: List of supported devices.
last_modified_at: '2020-03-19 09:53:43 +0800'
toc: true
toc_label: Contents
---

As of March 2020, this workshop and the different labs are designed to work on ESP32 based devices. Hopefully with the support of the community we will be able to onboard more devices.

Given we are using different devices not all of the lab capabilities are supported. The following table hopefully outlines the capabilities of a device vs. another.

**Note:** By default, the labs are designed to run on the M5StickC device. To change the device on which to run, you must edit the **{{ site.data.globals['code'].lab-config-file }}** file with the appropriate `#define` value.
{: .notice--info}


| Device | #define | Supported capabilities |
| ------        | ------- |  ------- |
| [M5StickC](https://m5stack.com/collections/m5-core/products/stick-c?variant=17203451265114) | `#define {{ site.data.globals['device-config'].m5stickc }}` | The M5StickC is an ESP32 Pico based device. It has an `accelerometer`, `screen` and 2 `buttons` |
| [ESP32DevkitC](https://www.espressif.com/en/products/hardware/esp32-devkitc/overview) | `#define {{ site.data.globals['device-config'].esp32-devkitc }}` | The ESP32 DevkitC is an ESP32 based device. It only has 1 `button` |
| Following devices are currently WIP: |
| ------        | ------- |  ------- |
| [M5Stack](https://m5stack.com/collections/m5-core/products/basic-core-iot-development-kit?variant=16804801937498) | `#define {{ site.data.globals['device-config'].m5stack }}` | The M5Stack is an ESP32 based device. It has a `screen` and 2 `buttons` |




