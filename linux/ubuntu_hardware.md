```bash
# detailed information about your RAM
# Look for the "Speed" field in the output, which indicates the RAM speed in MT/s (megatransfers per second) or MHz.
sudo dmidecode -t memory



# cannot turn on bluetooth?
# Try manually loading the Bluetooth modules again:
sudo modprobe -r btusb
sudo modprobe btusb
```
