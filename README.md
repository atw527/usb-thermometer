usb-thermometer: TEMPer1 and TEMPer1F
===============

### From Pete Chapman:

* Fixed poor precision for TEMPer1F (iProduct string "TEMPer1F_V1.3").

### From Peter Vojtek:

The original version of pcsensor0.0.1 was located on [this page](http://bailey.st/blog/2012/04/12/dirt-cheap-usb-temperature-sensor-with-python-sms-alerting-system/). I took the source code and fixed following bug:

* temperatures below zero overflow: 254.3 C is displayed instead of -1.3 C, . 

### Download & Build

``` bash
#[user]$
sudo apt-get install libusb-dev
git clone git clone git@github.com:atw527/usb-thermometer.git
cd usb-thermometer
make
```

### Run Permissions

If attempting to run the script as-is, an error message will be returned:

``` bash
#[user]$
./pcsensor
Could not set configuration 1
```

This can be fixed by running as root (workaround):

``` bash
#[user]$
sudo ./pcsensor
2017/12/24 10:01:12 Temperature 62.38F 16.88C
```

The better solution is to use the udev rules file to allow all users to access this usb device.

``` bash
#[user]$
sudo cp 99-tempsensor.rules /etc/udev/rules.d/99-tempsensor.rules
```

After installing the rules file, disconnect/reconnect the Temper1f or reboot the machine.  Now it will run at user-level.


``` bash
#[user]$
./pcsensor
2017/12/24 10:03:55 Temperature 63.05F 17.25C
```

### Nagios Perf Data Output

Run with `-n` to output Nagios performance data.

``` bash
#[user]$
./pcsensor -n
Temperature 63.28F, 17.38C | temperature_f=63.28; temperature_c=17.38
```
