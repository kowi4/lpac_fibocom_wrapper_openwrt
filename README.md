# LPAC Fibocom Wrapper for OpenWrt

A wrapper for [LPAC](https://github.com/estkme-group/lpac) designed to work with Fibocom FM350-GL modem, written in ash shell for OpenWrt systems.

## Disclaimer

This wrapper is an experimental implementation created with significant AI assistance, as the author has limited experience with ash scripting. The following commands have been tested:
- `lpac_wrapper profile list`
- `lpac_wrapper chip info`
- `lpac_wrapper profile download`
- `lpac_wrapper notification process`

Full functionality is not guaranteed. Use at your own risk.

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

Example commands:
```bash
lpac_wrapper chip info
lpac_wrapper profile list
lpac_wrapper profile download -a 'LPA:1$rsp.truphone.com$QR-G-5C-1LS-1W1Z9P7'
lpac_wrapper profile disable 1234567890
lpac_wrapper profile enable 1234567890
lpac_wrapper notification process -a -r
```

## Fixing libcurl Issues

Some commands (like profile download) may fail with default OpenWrt libcurl installation (https://github.com/openwrt/packages/issues/24963). To fix this:

### Step 1: Install libcurl-gnutls
```bash
opkg install libcurl-gnutls4
```

### Step 2: Check library paths
```bash
ls -l /usr/lib/libcurl*
```
You should see something like:
```
lrwxrwxrwx libcurl.so.4 -> libcurl.so.4.5.0      (default version)
-rwxr-xr-x libcurl-gnutls.so.4.5.0               (GnuTLS version)
```

### Step 3: Change libcurl.so.4 symlink
```bash
cd /usr/lib
rm libcurl.so.4
ln -s libcurl-gnutls.so.4 libcurl.so.4
```

### Reverting changes
If something goes wrong, restore the original link:
```bash
cd /usr/lib
rm libcurl.so.4
ln -s libcurl.so.4.5.0 libcurl.so.4
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

3. If experiencing libcurl issues, follow the libcurl fix instructions above

## Documentation

For comprehensive understanding of LPAC functionality, please review the official documentation:  
https://github.com/estkme-group/lpac/blob/main/docs/USAGE.md
