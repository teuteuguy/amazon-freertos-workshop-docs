---
title: "Lab 1 - AWS IoT Button"
excerpt: "Create your own AWS IoT Button, using the M5StickC!"
toc: true
toc_label: Lab 1 Contents
toc_sticky: true
---

A couple years ago, AWS announced the AWS IoT Button you could order from Amazon.com. Why not create our own?

For this lab, our M5StickC will act as an AWS IoT Button, allowing you to trigger downstream AWS Services.

## Configuration

In order to configure the code and your M5StickC for Lab1, you must change the following lines of code:

In file: **{{ site.data.globals['code'].lab-config-file }}**

```bash
Replace:
#define LABCONFIG_LAB0_SETUP

With:
#define LABCONFIG_LAB1_AWS_IOT_BUTTON
```

## Rebuild Code

```bash
cd {{ site.data.globals['code'].workshop }}
cmake --build build
```

## Flash Code

Please follow the same procedure from [Flash the firmware files]({{ "/labs/00-setup-the-labs/#flash-the-firmware-files" | relative_url }}).

## Result

What is going on, it's not working? Could we have forgotten something? :)

The serial console returns the following error message:

```bash
...
E (15866) lab_connection: Failed to initialize the MQTT Connection: 1
E (15896) lab_connection: Exiting
...
```

Could our IoT Policy be **incomplete**? <button id="incomplete-policy" class="challenge btn btn--info btn--small">Answer</button>
{% capture incomplete-policy-answer %}
If you recall when we set up our IoT Policy for our Thing we created it quite open, true, but we forgot an important Allow. Our IoT Policy **Allowed** Publishing, Subscribing and Receiving, but it did not allow Connecting! Go edit your policy, and add the following so that the document looks like:
```bash
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Connect",
        "iot:Publish",
        "iot:Subscribe",
        "iot:Receive"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```

Reset the device, the device should connect without any problems.

**Note:** if you are using the M5StickC, the side button, just side of the screen can be held for a couple seconds and the device will reboot.
{% endcapture %}
<div id="incomplete-policy-answer" class="notice--info hide">
  {{ incomplete-policy-answer | markdownify }}
</div>

Log into your AWS IoT Management console, use the web MQTT client and subscribe to the following topic:

```bash
mydevice/+
```

Click your M5StickC button: You should see the following JSON document flow on the broker

```json
{
	"serialNumber": "[YOUR DEVICE SERIAL NUMBER]",
	"clickType": "SINGLE"
}
```

Hold your M5StickC button: You should see the following JSON document flow on the broker

```json
{
	"serialNumber": "[YOUR DEVICE SERIAL NUMBER]",
	"clickType": "HOLD"
}
```

## Done

You are done with Lab 1.

## Knowledge check
* Q1: **mydevice/+**, what does the **"+"** do? <button id="q1" class="challenge btn btn--info btn--small">Answer</button>
{% capture q1-answer %}
The <strong>"+"</strong> in the topic name, acts as a wildcard for that given section of the topic. There are 2 topic wildcards that can be used:

| Wildcard | Description |
| :------: | ----------- |
| **#**	| Must be the last character in the topic to which you are subscribing. Works as a wildcard by matching the current tree and all subtrees. For example, a subscription to **Sensor/#** receives messages published to **Sensor/**, **Sensor/temp**, **Sensor/temp/room1**, but not the messages published to **Sensor**. |
| **+** | Matches exactly one item in the topic hierarchy. For example, a subscription to **Sensor/+/room1** receives messages published to **Sensor/temp/room1**, **Sensor/moisture/room1**, and so on. |
{% endcapture %}
<div id="q1-answer" class="notice--info hide">
  {{ q1-answer | markdownify }}
</div>

* Q2: What can you replace **"+"** with? <button id="q2" class="challenge btn btn--info btn--small">Answer</button>
{% capture q2-answer %}
In our IoT Button use-case, we saw that the button publishes to a topic that has **m5stickc/[YOUR DEVICE SERIAL NUMBER]**. In the MQTT client, if you subscribe to that specific topic (ie. **mydevice/[YOUR DEVICE SERIAL NUMBER]**), you will see the data coming only from your device.
{% endcapture %}
<div id="q2-answer" class="notice--info hide">
  {{ q2-answer | markdownify }}
</div>

## Challenge
* Can you create an AWS IoT Rule that gets triggered on a button press and sends your cell phone an SMS? <button id="c1" class="challenge btn btn--info btn--small">Answer</button>
{% capture c1-answer %}
This is achieved by first creating an AWS SNS Topic, creating a subscription to an SMS phone number. Here your phone number.
Then, create an AWS IoT Rule with the following SQL statement:
```bash
SELECT * FROM 'mydevice/#'
```
Then select the AWS SNS topic you created as the action for the rule.
{% endcapture %}
<div id="c1-answer" class="notice--info hide">
  {{ c1-answer | markdownify }}
</div>
* Can you make that rule ONLY send you an SMS if you HOLD the button? <button id="c2" class="challenge btn btn--info btn--small">Answer</button>
{% capture c2-answer %}
This is achieved by modifying your rule's SQL query to have a where clause. Notice how when you click, the JSON object you receive has **"clickType": "SINGLE"** and when you hold the button, you receive **"clickType": "HOLD"**. Simply modify your SQL statement with:
```bash
SELECT * FROM 'mydevice/#' WHERE clickType = 'HOLD'
```
{% endcapture %}
<div id="c2-answer" class="notice--info hide">
  {{ c2-answer | markdownify }}
</div>
* Can you modify the AWS IoT Policy you created in prior steps to limit communications to only the required topics? <button id="c3" class="challenge btn btn--info btn--small">Answer</button>
{% capture c3-answer %}
This is achieved by being more prescriptive on the topics that your device is allowed to publish to. For example, currently our device publishes on a topic like: **mydevice/d8a01d551b1c**. In that case, lets add that topic to the policy like so:
```bash
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iot:Connect",
        "iot:Subscribe",
        "iot:Receive"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "iot:Publish",
      ],
      "Resource": [
        "arn:aws:iot:[REGION]:[AWS ACCOUNT ID]:topic/mydevice/d8a01d551b1c"
      ]
    }
  ]
}
```
**Note:** Make sure to replace **REGION** and **AWS ACCOUNT ID** with your specific region and account id.
{% endcapture %}
<div id="c3-answer" class="notice--info hide">
  {{ c3-answer | markdownify }}
</div>