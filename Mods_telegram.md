###Mods for insert Telegram bot on current release

*Insert file telegram.py (https://github.com/py1tcm/radiosonde_auto_rx/blob/Telegram/auto_rx/autorx/telegram.py) in /radiosonde_auto_rx/auto_rx/autorx*

**config.py**

above line 68 (# SDR Settings) insert:

~~~bash
# Telegram Settings
        "telegram_enabled" : False,
        "telegram_bot_token" : None,
        "telegram_chat_id" : None,
        # Telegram Landing Settings
        "telegram_landing_enabled" : False,
        "telegram_landing_lat1" : 0.0,
        "telegram_landing_lon1" : 0.0,
        "telegram_landing_alt1" : 0.0,
        "telegram_landing_distance1" : 0.0,
        "telegram_landing_altitude1" : 0.0,

~~~

above line 226 (# SDR Settings) insert:

~~~bash
        # Telegram Settings
        if config.has_option("telegram", "telegram_enabled"):
            try:
                auto_rx_config["telegram_enabled"] = config.getboolean("telegram", "telegram_enabled")
                auto_rx_config["telegram_bot_token"] = config.get("telegram", "telegram_bot_token")
                auto_rx_config["telegram_chat_id"] = config.get("telegram", "telegram_chat_id")
            except:
                logging.error("Config - Invalid telegram settings. Disabling.")
                auto_rx_config["telegram_enabled"] = False

        if config.has_option("telegram_landing", "telegram_landing_enabled"):
            try:
                auto_rx_config["telegram_landing_enabled"] = config.getboolean("telegram_landing", "telegram_landing_enabled")
                auto_rx_config["telegram_landing_lat1"] = config.getfloat("telegram_landing", "telegram_landing_lat1")
                auto_rx_config["telegram_landing_lon1"] = config.getfloat("telegram_landing", "telegram_landing_lon1")
                auto_rx_config["telegram_landing_alt1"] = config.getfloat("telegram_landing", "telegram_landing_alt1")
                auto_rx_config["telegram_landing_distance1"] = config.getfloat("telegram_landing", "telegram_landing_distance1")
                auto_rx_config["telegram_landing_altitude1"] = config.getfloat("telegram_landing", "telegram_landing_altitude1")
            except:
                logging.error("Config - Invalid telegram landing settings. Disabling.")
                auto_rx_config["telegram_landing_enabled"] = False

~~~

**auto_rx.py**

above line 31 (from autorx.habitat import HabitatUploader) insert:

~~~bash
from autorx.telegram import TelegramNotification

~~~

above line 881 (if config["email_enabled"]:) insert:

~~~bash
   if config["telegram_enabled"]:
        _telegram_notification = TelegramNotification(
            bot_token = config["telegram_bot_token"],
            chat_id = config["telegram_chat_id"],
            landing_lat1 = config["telegram_landing_lat1"],
            landing_lon1 = config["telegram_landing_lon1"],
            landing_alt1 = config["telegram_landing_alt1"],
            landing_distance1 = config["telegram_landing_distance1"],
            landing_altitude1 = config["telegram_landing_altitude1"],
            timeout = config["rx_timeout"]
	)

        exporter_objects.append(_telegram_notification)
        exporter_functions.append(_telegram_notification.add)

~~~

**station.cfg.example**

above line 410 (# ROTATOR CONTROL #) insert:

~~~bash
##########################
# TELEGRAM NOTIFICATIONS #
##########################
# Sends telegram notification to specified chat group when a new Sonde is detected
# See https://techthoughts.info/how-to-create-a-telegram-bot-and-send-messages-via-api/ for details.
[telegram]
telegram_enabled = False
telegram_bot_token = TELEGRAM_BOT_TOKEN
telegram_chat_id = TELEGRAM_CHAT_ID
# Define a location so that a telegram notification is sent when the balloon is descending and
# is within desired range and altitude of this location.  One location currently.  Additional locs to be added later.
[telegram_landing]
telegram_landing_enabled = False
# The location to watch
telegram_landing_lat1 = 0.0
telegram_landing_lon1 = 0.0
telegram_landing_alt1 = 0.0
# Only get a notification when the balloon is within this range and this altitude. Distance and altitude in meters.
telegram_landing_distance1 = 30000
telegram_landing_altitude1 = 2000

~~~