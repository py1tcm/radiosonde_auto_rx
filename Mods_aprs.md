
### Mods to bypass aprs inhibition on current release ###

**config.py**

*comment (""") above line 686 (ALLOWED_APRS_SERVERS = ["radiosondy.info"]) until after line 699 (auto_rx_config["aprs_port"] = 14590)*


**aprs.py**

line 507 ( _packet = "%s>APRARX,SONDEGATE,TCPIP,qAR,%s:%s\r\n" % ( ) modify for:

~~~bash
_packet = "%s>APRS,SONDEGATE,TCPIP,qAR,%s:%s\r\n" % (

~~~

**station.cfg / station.cfg.example**

line 264 ( aprs_server = radiosondy.info ) modify for:

~~~bash
aprs_server = rotate.aprs2.net

~~~

line 217 ( aprs_port = 14590 ) modify for:

~~~bash

aprs_port = 14580

~~~