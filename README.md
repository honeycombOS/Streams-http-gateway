# Streams-WiFi-Gateway

## Preparation
Install rust if you don't have it already, find the instructions here https://www.rust-lang.org/tools/install

Make sure you also have the build dependencies installed, if not run:  
`sudo apt install build-essential`  
`sudo apt install pkg-config`  
`sudo apt install libssl-dev`  
`sudo apt update`  

## Installing the streams-gateway

Download the Repository:  

`git clone https://github.com/iot2tangle/Streams-wifi-gateway.git`
  
Configure the streams-gateway:  

`nano config.json`  
 
Set the *device_names* to whitelist the values specified in the configuration file of the Devices.  
Change *port, node, mwm, local_pow* if needed 


  
## Runnig the Examples:  
  
Run the Streams Gateway:  
`cargo run --release`  
This starts the server which will forward messages from the XDK to the Tangle  
  
The Output will be something like this:  
`>> Starting.... `  
`>> Channel root: "ab3de895ec41c88bd917e8a47d54f76d52794d61ff4c4eb3569c31f619ee623d0000000000000000"`  

`>> To read the messages copy the channel root into http://iot2tangle.link/ `
  
`>> Listening on http://0.0.0.0:8080`    

To send data to the server you can use Postman, or like in this case cURL, make sure the port is the same as in the config.json file:  
`  
curl --location --request POST '127.0.0.1:8080/sensor_data'   
--header 'Content-Type: application/json'   
--data-raw '{
    "iot2tangle": [
        {
            "sensor": "Gyroscope",
            "data": [
                {
                    "x": "4514"
                },
                {
                    "y": "244"
                },
                {
                    "z": "-1830"
                }
            ]
        },
        {
            "sensor": "Acoustic",
            "data": [
                {
                    "mp": "1"
                }
            ]
        }
    ],  
    "device": "DEVICE_ID_1",  
    "timestamp": 1558511111  
}'  
`  
Note: If the "timestamp" value is set to 0 a new timestamp will be added by the realy server before the data is published to the Tangle.

         
IMPORTANT: The device will be authenticated through the "device" field in the request (in this case XDK_HTTP), this has to match what was set as device_name in the config.json on the Gateway (see Configuration section above)!  
  
After a few seconds you should now see the data beeing recieved by the Subscriber!