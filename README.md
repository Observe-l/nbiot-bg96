# nbiot-bg96
Based on "Sixfab Cellular IoT App. shield"

### Some useful link

1. Sixfab RPi source code: [GitHub Pages](https://github.com/sixfab/Sixfab_RPi_CellularIoT_App_Shield)
2. BG96 AT command: [User manual](https://docs.particle.io/assets/pdfs/Quectel_BG96_AT_Commands_Manual_V2.1.pdf)
3. M2M AT command: [Some examples](https://m2msupport.net/m2msupport/at-commands-to-get-device-information/)
4. TCP/IP: [BG96 AT commands](https://sixfab.com/wp-content/uploads/2018/09/quectel_bg96_tcpip_at_commands_manual_v1-1.pdf)
5. MQTT: [BG96 MQTT note](https://www.quectel.com/wp-content/uploads/2021/03/Quectel_BG96_MQTT_Application_Note_V1.2.pdf)



#### Initialize the Sixfab App shield. (M1 NB-IoT SIM card)

``` python
'''
NOTE!!!! only support RPi OS(32bit or 64bit), cannot use Ubuntu
'''
# Sixfab API
from cellulariot import cellulariot
import time


node = cellulariot.CellularIoTApp()
node.setupGPIO()
# Initialize the BG96 model
node.disable()
time.sleep(0.5)
node.enable()
time.sleep(0.5)
node.powerUp()

# Test the ATCommand. Ubuntu didn't support this command
node.sendATComm("ATE1","OK\r\n")
# Get some basic information
node.getIMEI()
node.getFirmwareInfo()
node.getHardwareInfo()

# Dissable SIM card before we confige BG96
node.sendATComm("AT+CFUN=0","OK\r\n")
# Set up APN. My provider is Singapor M1.
node.sendATComm("AT+CGDCONT=1,\"IP\",\"m1-nbiot\"","OK\r\n")
# Activate SIM card
node.sendATComm("AT+CFUN=1","OK\r\n")

# Set up the Band. I use nb-iot sim cart, so my mode is: CATNB1_MODE
node.setGSMBand(node.GSM_900)
time.sleep(0.5)
node.setCATM1Band(node.LTE_B5)
time.sleep(0.5)
node.setNBIoTBand(node.LTE_CATNB1_ANY)
time.sleep(0.5)
node.getBandConfiguration()
time.sleep(0.5)  
node.setMode(node.CATNB1_MODE)
time.sleep(0.5)

# Check the connection and signalquality
node.connectToOperator()
time.sleep(0.5)
node.getSignalQuality()
time.sleep(0.5)
node.getQueryNetworkInfo()
time.sleep(0.5)

# If connect successful, we can get a dynamic IP address
node.sendATComm("AT+CGPADDR","OK\r\n")

# Enable TCP service
# node.deactivateContext()
# time.sleep(0.5)
# node.activateContext()
# time.sleep(0.5)

# Enable UDP service
# node.closeConnection()
# time.sleep(0.5)
# node.startUDPService()
# time.sleep(0.5)
```

