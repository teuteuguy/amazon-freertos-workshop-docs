---
title: "AWS IoT Kickstart"
excerpt: "Connect the M5StickC to the AWS IoT Kickstart solution."
toc: true
toc_label: Contents
toc_sticky: true
---

In this integration we will setup and install the [AWS IoT Kickstart](https://github.com/aws-samples/aws-iot-kickstart) (aka. Sputnik) solution, and configure it to visualize and interact with your M5StickC.

**Disclaimer:** At the time or writing, Sputnik does not support OTA update (deployment) of device code onto Amazon FreeRTOS devices. For this tutorial, we will simply create a device blueprint for the M5StickC AirCon lab and show you can simply visualize data from such devices.
{: .notice--info}

## Step 1 - Setup your device

This tutorial will leverage our Air Conditioning simulator on the M5StickC. For this, please first make sure you are running [Lab 2 - Thing Shadow]({{ '/labs/02-thing-shadow' | relative_url }}) successfully.


## Step

```bash
{
    "View": {
        "subscriptions": {
            "shadowGetAccepted": "$aws/things/[THING_NAME]/shadow/get/accepted",
            "shadowUpdateAccepted": "$aws/things/[THING_NAME]/shadow/update/accepted"
        },
        "widgets": [
            {
                "data": {
                    "text": [
                        {
                            "data": {
                                "value": "State:"
                            },
                            "type": "text",
                            "class": "col-6"
                        },
                        {
                            "data": {
                                "output": "$aws/things/[THING_NAME]/shadow/update",
                                "input": [
                                    "shadowUpdateAccepted",
                                    "shadowGetAccepted"
                                ],
                                "toggleTrue": 1,
                                "toggleFalse": 0,
                                "initWithShadow": true,
                                "value": {
                                    "output": "state.desired.powerOn",
                                    "input": "state.desired.powerOn"
                                }
                            },
                            "type": "checkbox",
                            "class": "col-6 pull-right"
                        }
                    ],
                    "title": [
                        {
                            "data": {
                                "value": "Air Conditioner"
                            },
                            "type": "text",
                            "class": "col-12"
                        }
                    ]
                },
                "type": "card",
                "class": "col-xl-3 col-md-6 col-sm-12"
            },
            {
                "data": {
                    "text": [
                        {
                            "data": {
                                "input": [
                                    "shadowUpdateAccepted",
                                    "shadowGetAccepted"
                                ],
                                "minValue": 0,
                                "initWithShadow": true,
                                "maxValue": 50,
                                "value": "state.reported.temperature"
                            },
                            "type": "gauge",
                            "class": "col-12"
                        }
                    ],
                    "title": [
                        {
                            "data": {
                                "value": "Temperature"
                            },
                            "type": "text",
                            "class": "col-12"
                        }
                    ]
                },
                "type": "card",
                "class": "col-xl-3 col-md-6 col-sm-12"
            },
            {
                "data": {
                    "text": [
                        {
                            "data": {
                                "input": [
                                    "shadowUpdateAccepted"
                                ],
                                "title": "Temperature",
                                "value": "state.reported.temperature"
                            },
                            "type": "graph-realtime",
                            "class": "col-12"
                        }
                    ],
                    "title": [
                        {
                            "data": {
                                "value": "Temperature"
                            },
                            "type": "text",
                            "class": "col-12"
                        }
                    ]
                },
                "type": "card",
                "class": "col-xl-6 col-lg-12"
            }
        ]
    }
}
```