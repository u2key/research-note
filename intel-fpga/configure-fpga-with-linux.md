# Configure FPGA circuit with quartus on Linux PC

## 1. Configure udev rule

```
sudo vim /etc/udev/rules.d/51-usbblaster.rules
```
```
SUBSYSTEM=="usb", ATTR{idVendor}=="09fb", ATTR{idProduct}=="6001", MODE="0666"
SUBSYSTEM=="usb", ATTR{idVendor}=="09fb", ATTR{idProduct}=="6002", MODE="0666"
SUBSYSTEM=="usb", ATTR{idVendor}=="09fb", ATTR{idProduct}=="6003", MODE="0666"
SUBSYSTEM=="usb", ATTR{idVendor}=="09fb", ATTR{idProduct}=="6010", MODE="0666"
SUBSYSTEM=="usb", ATTR{idVendor}=="09fb", ATTR{idProduct}=="6810", MODE="0666"
```

## References

- https://www.intel.co.jp/content/www/jp/ja/support/programmable/support-resources/download/dri-usb-b-lnx.html
