# LPAC Fibocom Wrapper for OpenWrt
It is a wrapper for LPAC (https://github.com/estkme-group/lpac) designed to work with Fibocom FM350 GL written in ash for OpenWrt


# Disclaimer 
This wrapper is far from perfect written with huge AI help as I don't know ash at all, I only tested basic readonly commands `lpac_wrapper profile list` and `lpac_wrapper chip info`, it might not work for you. Use with caution.
It works similar as Windows version designed here https://github.com/prusa-dev/lpac-fibocom-wrapper/tree/main.

# Before you try
1. I am not responsible if this will break your modem, use at your own risk.
2. Make sure you have esim enabled: `AT+GTDUALSIM=1`
3. Make sure you have correct AT_DEVICE configured for wrapper: `export AT_DEVICE="/dev/ttyUSBx"`
4. Make sure your modem is in correct state and there is no other scripts using AT port (powercycle/reboot usually works)

# Installation
`wget https://raw.githubusercontent.com/kowi4/lpac_fibocom_wrapper_openwrt/refs/heads/main/lpac_wrapper -O /usr/bin/lpac_wrapper`
`chmod +x /usr/bin/lpac_wrapper`

# Dependencies
`opkg update && opkg install lpac jq sexpect sms-tool`

# Configure lpac to work with wrapper
`uci set lpac.global.apdu_backend='stdio' && uci set lpac.global.apdu_debug='0' && uci commit lpac`

# Confiigure AT Port for wrapper
By default there is AT_DEVICE="/dev/ttyUSB3" 
to configure different port use:
`export AT_DEVICE="/dev/ttyUSBx"`

# Run wrapper
It works with stdio so you will see all APDU messages in the console. The last one should be LPA which is the real result from LPAC.
`lpac_wrapper chip info`

# Debugging 
If doesn't work you can check the logs:
`cat /tmp/lpac_wrapper.log`
or enable lpac debugging 
`uci set lpac.global.apdu_debug='0' && uci commit lpac`

# Make sure to read the LPAC documentation first to understand what you are doing
https://github.com/estkme-group/lpac/blob/main/docs/USAGE.md
