
###Mods for bypass aprs inibith on current release

**config.py**

*comment (""") above line 686 (ALLOWED_APRS_SERVERS = ["radiosondy.info"]) until after line 699 (auto_rx_config["aprs_port"] = 14590)*


**aprs.py**

line 507 ( _packet = "%s>APRARX,SONDEGATE,TCPIP,qAR,%s:%s\r\n" % ( ) modify for:

~~~bash
_packet = "%s>APRS,SONDEGATE,TCPIP,qAR,%s:%s\r\n" % (

~~~