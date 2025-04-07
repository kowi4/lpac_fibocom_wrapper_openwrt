# LPAC Fibocom Wrapper for OpenWrt

A wrapper for [LPAC](https://github.com/estkme-group/lpac) designed to work with Fibocom FM350-GL modem, written in ash shell for OpenWrt systems.

## Disclaimer

This wrapper is an experimental implementation created with significant AI assistance, as the author has limited experience with ash scripting. Basic read-only commands (`lpac_wrapper profile list`, `lpac_wrapper chip info`) have been tested, but full functionality is not guaranteed. Use at your own risk.

The wrapper follows similar functionality to the Windows version available at:  
https://github.com/prusa-dev/lpac-fibocom-wrapper/tree/main

## Installation

```bash
wget https://raw.githubusercontent.com/kowi4/lpac_fibocom_wrapper_openwrt/main/lpac_wrapper -O /usr/bin/lpac_wrapper
chmod +x /usr/bin/lpac_wrapper
```

## Dependencies

```bash
opkg update && opkg install lpac sexpect sms-tool
```

## Optional Dependencies
The following package is optional and only used for pretty-printing the LPA response at the end of command execution:
```bash
opkg update && opkg install jq
```

## Configuration

### LPAC Configuration
```bash
uci set lpac.global.apdu_backend='stdio'
uci set lpac.global.apdu_debug='0'
uci commit lpac
```

### AT Port Configuration
Default AT device: `/dev/ttyUSB3`  
To specify a different port:
```bash
export AT_DEVICE="/dev/ttyUSBx"
```

## Usage

The wrapper operates via stdio, displaying all APDU messages in the console. The final output will be the LPA result from LPAC.

Example:
```bash
lpac_wrapper chip info
```

## Troubleshooting

1. Check wrapper logs:
   ```bash
   cat /tmp/lpac_wrapper.log
   ```

2. Enable LPAC debugging:
   ```bash
   uci set lpac.global.apdu_debug='1' && uci commit lpac
   ```

## Documentation

For comprehensive understanding of LPAC functionality, please review the official documentation:  
https://github.com/estkme-group/lpac/blob/main/docs/USAGE.md
