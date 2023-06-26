---
title: Send Data to SQL
date: 2023-05-05
hero: "/images/4.jpeg"
excerpt: How to use SQL to store data
authors:
  - Hugo Authors

---


## Task - send GPS data to an IoT Hub

1. Create a new IoT Hub using the free tier.

⚠️ You can refer to the [instructions for creating an IoT Hub from project 2, lesson 4](../../../2-farm/lessons/4-migrate-your-plant-to-the-cloud/README.md#create-an-iot-service-in-the-cloud) if needed.

Remember to create a new Resource Group. Name the new Resource Group `gps-sensor`, and the new IoT Hub a unique name based on `gps-sensor`, such as `gps-sensor-<your name>`.

💁 If you still have your IoT Hub from the previous project, you can re-use it. Remember to use the name of this IoT Hub and the Resource Group it is in when creating other services.

1. Add a new device to the IoT Hub. Call this device `gps-sensor`. Grab the connection string for the device.

1. Update your device code to send the GPS data to the new IoT Hub using the device connection string from the previous step.

⚠️ You can refer to the [instructions for connecting your device to an IoT from project 2, lesson 4](../../../2-farm/lessons/4-migrate-your-plant-to-the-cloud/README.md#connect-your-device-to-the-iot-service) if needed.

1. When you send the GPS data, do it as JSON in the following format:

    ```json
    {
        "gps" :
        {
            "lat" : <latitude>,
            "lon" : <longitude>
        }
    }
    ```

1. Send GPS data every minute so you don't use up your daily message allocation.

If you are using the Wio Terminal, remember to add all the necessary libraries, and set the time using an NTP server. Your code will also need to ensure that it has read all the data from the serial port before sending the GPS location, using the existing code from the last lesson. Use the following code to construct the JSON document:

```cpp
DynamicJsonDocument doc(1024);
doc["gps"]["lat"] = gps.location.lat();
doc["gps"]["lon"] = gps.location.lng();
```

If you are using a Virtual IoT device, remember to install all the needed libraries using a virtual environment.

For both the Raspberry Pi and Virtual IoT device, use the existing code from the last lesson to get the latitude and longitude values, then send them in the correct JSON format with the following code:

```python
message_json = { "gps" : { "lat":lat, "lon":lon } }
print("Sending telemetry", message_json)
message = Message(json.dumps(message_json))
```

 💁 You can find this code in the [code/wio-terminal](code/wio-terminal), [code/pi](code/pi) or [code/virtual-device](code/virtual-device) folder.

Run your device code and ensure messages are flowing into IoT Hub using the `az iot hub monitor-events` CLI command.