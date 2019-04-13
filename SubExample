import network
from umqtt.robust import MQTTClient #no esta incluida en el firmware para el esp32, deben ser agregados los scripts de mqtt
import time

def cb(topic, msg):
    print (msg)

#conexion wifi
sta_if = network.WLAN(network.STA_IF)
sta_if.active(True)
sta_if.connect("SSID", "PASS")
time.sleep(5)

#se debe especificar el token de acuerdo a la cuenta de ubidots y el clientid con el que se subscribira el esp
ubidotsToken = "----"
clientID = "----"
topic="/v1.6/devices/nombre_dispositivo/variable/lv" #el topic define a que device en especifico es que se va a subir datos
                                 #b"/v1.6/devices/{NOMBRE_DISPOSITIVO}" en el que NOMBRE_DISPOSITIVO es quien
                                 #define entre los devices creados al cual se quiere subir el dato

client = MQTTClient(clientID, "industrial.api.ubidots.com", 1883, user = ubidotsToken, password = ubidotsToken) #creacion de objeto
client.connect() #conexion a ubidots
client.set_callback(cb)                    
client.subscribe(bytes(topic, 'utf-8'))

while True:
    try:
        client.wait_msg()
        
    except KeyboardInterrupt:
        print('Ctrl-C pressed...exiting')
        client.disconnect()
        sys.exit()
