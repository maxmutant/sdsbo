# SDSBO
Simple dwm status bar for OpenBSD

![screenshot](https://raw.githubusercontent.com/MaxMutantMayer/sdsbo/master/screenshot.png)

## Features
* Multiple modules for customization
* Module label customization - Use icons or plain text
* Color support (dwm-status2d patch or similar is required)
* Easy configuration via config file

## Modules
* Battery - Lifetime and status via `apm`
* Date - In specified format via `date`
* IPv4 - inet address of specified interface via `ifconfig`
* IPv6 - inet6 address of specified interface via `ifconfig`
* Temperature - °C of specified device via `sysctl hw.sensors`
* Time - In specified format via `date`
* Volume - Master volume and status via `mixerctl`

## Install and Use
```
git clone https://github.com/MaxMutantMayer/sdsbo.git
cd sdsbo
doas chmod +x sdsbo
./sdsbo
```
Alternatively, you can start sdsbo with a custom configuration (see [Customization](#Customization) bellow):
```
./sdsbo -c /path/to/config
```
If you made sure everything works fine and you would like to display the bar when X is started, you can add the following to your `~/.xsession` file (assuming script is located in `~/.local/bin`):
```
# start sdsbo with ~/.config/sdsbo/config configuration file
"$HOME"/.local/bin/sdsbo -c "$HOME"/.config/sdsbo/config &

# use this instead to run only one sdsbo instance after killing + restarting dwm
# [ -z "$(pgrep -fl sdsbo)" ] && \
#	"$HOME"/.local/bin/sdsbo -c "$HOME"/.config/sdsbo/config &

# start dwm
dwm
```

## Customization
You have two options to customize your bar:
1. Modify the default values directly within the sdsbo script
2. Override the defaults within a config file and use the `-c` option

### Default Values
```
# Colors
LC=					# label color
TC=					# text color

# Labels
LBAT1=CHA			# charging label
LBAT2=BAT			# battery label
LDATE=DATE			# date label
LIPV4=IPv4			# IPv4 label
LIPV6=IPv6			# IPv6 label
LTEMP=TEMP			# temperature label
LTIME=TIME			# time label
LVOL1=VOL			# volume 'muted' label
LVOL2=VOL			# volume label

# Format strings
FDATE="+%Y-%m-%d"	# date format string
FTIME="+%T"			# time format string

# Interfaces and devices
IPIF=lo0			# IP address interface
DTEMP=cpu0			# temperature device
```

### Enable/Disable modules
Specify the modules you want to display by overriding the `mods()` function:
```
# use all modules
mods() {
	echo "$(mod_temp) $(mod_ipv4) $(mod_ipv6) $(mod_vol) $(mod_bat) $(mod_date) $(mod_time)"
}
```

### Icons instead of text labels
In order to have icons within your status bar, you have to make sure that dwm is configured to use a font that actually supports your inserted icons.

If you would like to use similar icons as shown in the screenshot, you would have to set the labels to [Font Awesome Icons](https://fontawesome.com/icons) and configure dwm to use the FontAwesome font (`config(.def).h`):
```
/* use monospace 10 as default and FontAwesome 10 as fallback */
static const char *fonts[] = { "monospace:size=10", "FontAwesome:size=10" };
```

### Example
The `config.example` file represents an example configuration, which displays the current CPU temperature and time in the 24-hour format "HH:MM". Additionally, the labels are colored in blue (status2d patch required).
