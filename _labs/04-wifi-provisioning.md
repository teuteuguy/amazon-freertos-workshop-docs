---
title: "Lab 4 - Wifi Provisioning"
excerpt: "Provision wifi credentials on the device over BLE"
toc: true
toc_label: Lab 4 Contents
toc_sticky: true
---

In [Lab 0 - Setup the Labs]({{ "/labs/00-setup-the-labs" | relative_url }}) we hard coded the WIFI credentials onto our device by editing the **aws\_clientcredential.h** file and adding the WIFI SSID and Password:

```bash
...
#define clientcredentialWIFI_SSID            "[WILL BE PROVIDED]"
...
#define clientcredentialWIFI_PASSWORD        "[WILL BE PROVIDED]"
...
```

Now, lets use the BLE WIFI Provisioning capability that Amazon FreeRTOS provides.

## Configuration

**Note 1:** Out of pure simplicity, we won't modify the **aws\_clientcredential.h** file, but if you want you can replace the **clientcredentialWIFI_SSID** and **clientcredentialWIFI_PASSWORD** with empty strings **""**. But since we will be enabling BLE WIFI Provisioning, these defines will be ignored anyway.
{: .notice--info}

Now.

In order to configure the code for Lab4, all we have to do is enable BLE WIFI Provisioning by un-commenting the you must change the following lines of code:

In file: **{{ site.data.globals['code'].lab-config-file }}**

```bash
Replace:
// #define {{ site.data.globals['lab-config'].lab4 }}

With:
#define {{ site.data.globals['lab-config'].lab4 }}
```

## Rebuild Code

```bash
cd {{ site.data.globals['code'].workshop }}
cmake --build build
```

## Flash Code

Please follow the same procedure from [Flash the firmware files]({{ "/labs/00-setup-the-labs/#flash-the-firmware-files" | relative_url }}).

## Result

As you'll see from the terminal, the code now waits for BLE incoming connection in order to configure the wifi credentials.

```bash
...
4 185 [iot_thread] [INFO ][DEMO][1850] No networks connected for the demo. Waiting for a network connection....
```

You must download one of the 2 following applications for your mobile device.

[![iOS]({{ "/assets/lab-4/apple-app-store-badge.png" | relative_url }})]({{ site.data.globals['mobile-apps'].wifi-prov-ios }})[![Android]({{ "/assets/lab-4/google-play-badge.png" | relative_url }})]({{ site.data.globals['mobile-apps'].wifi-prov-android }})

**Note 2:** The above applications were built from the Amazon FreeRTOS Mobile SDK source code, removing the login requirements for the purpose of these labs. Obviously, we would recommend, for production devices, not only to authenticate your users, but also potentially pair your BLE devices for extra security.
{: .notice--info}

Once loaded the application should find your device:

![lab4-0]({{ "/assets/lab-4/lab4-0.png" | relative_url }})

Select your device:

![lab4-1]({{ "/assets/lab-4/lab4-1.png" | relative_url }})

And the terminal should show:

```bash
...
4 185 [iot_thread] [INFO ][DEMO][1850] No networks connected for the demo. Waiting for a network connection....
5 35117 [ble] [INFO ][DEMO][351170] BLE Connected to remote device, connId = 0

```

Enter the network configuration screen for your device:

![lab4-2]({{ "/assets/lab-4/lab4-2.png" | relative_url }})![lab4-3]({{ "/assets/lab-4/lab4-3.png" | relative_url }})

Select the WIFI network of your choice and fill out the password:

![lab4-4]({{ "/assets/lab-4/lab4-4.png" | relative_url }})

Your terminal should show the device connecting to the WIFI network:

```bash
...
I (466076) wifi: mode : sta (d8:a0:1d:55:1b:1c)
I (466076) WIFI: SYSTEM_EVENT_STA_START
GATT procedure initiated: notify; att_handle=81
GATT procedure initiated: notify; att_handle=81
GATT procedure initiated: notify; att_handle=81
GATT procedure initiated: notify; att_handle=81
GATT procedure initiated: notify; att_handle=81
GATT procedure initiated: notify; att_handle=81
GATT procedure initiated: notify; att_handle=81
GATT procedure initiated: notify; att_handle=81
GATT procedure initiated: notify; att_handle=81
GATT procedure initiated: notify; att_handle=81
I (538436) wifi: new:<1,0>, old:<1,0>, ap:<255,255>, sta:<1,0>, prof:1
I (539436) wifi: state: init -> auth (b0)
I (539436) wifi: state: auth -> assoc (0)
I (539446) wifi: state: assoc -> run (10)
I (539526) wifi: connected with MUYA Game Room, channel 1, BW20, bssid = 68:a8:6d:64:16:9b
I (539526) wifi: pm start, type: 1

I (539526) WIFI: SYSTEM_EVENT_STA_CONNECTED
6 53935 [wifi] Network buffers: 60 lowest 60
7 53935 [wifi] Heap: current 98696 lowest 85580
8 53935 [wifi] Queue space: lowest 65
9 53965 [wifi] Network buffers: 59 lowest 59
10 54001 [IP-task] Queue space: lowest 64
11 54334 [wifi] Network buffers: 60 lowest 58
12 54395 [IP-task] vDHCPProcess: offer c0a801b8ip
I (544306) event: sta ip: 192.168.1.184, mask: 255.255.255.0, gw: 192.168.1.1
I (544306) WIFI: SYSTEM_EVENT_STA_GOT_IP
13 54396 [IP-task] vDHCPProcess: offer c0a801b8ip
...
```

**Note3:** if you are using the M5StickC, there is a side button, pressing this side button will reboot the device.
A **long press** on the button will reset the stored wifi credentials on the device.
{: .notice--info}


## Done

You are done with Lab 4.
