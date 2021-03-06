Monitors network interfaces and shows current traffic in a textbox. 

```lua
mynet = lain.widgets.net()
```

### input table

Variable | Meaning | Type | Default
--- | --- | --- | ---
`timeout` | Refresh timeout seconds | int | 2
`iface` | Network device(s) | array (new api) or string (old api) | autodetected
`units` | Units | int | 1024 (kilobytes) 
`notify` | Display "no carrier" notifications | string | "on"
`screen` | Notifications screen | int | 1
`settings` | User settings | function | empty function

Possible other values for `units` are 1 (byte) or multiple of 1024: 1024^2 (mb), 1024^3 (gb), and so on.

If `notify = "off"` is set, the widget won't display a notification when there's no carrier.

`settings` can use the following `iface` related strings.

With the **old** API:

- `net_now.carrier` ("0", "1");
- `net_now.state` ("up", "down");
- `net_now.sent` and `net_now.received` (numbers).

With the **new** API:

- `net_now.sent` and `net_now.received` (numbers) will be the sum across all specified interfaces;
- `net_now.devices["interface"]` contains the same attributes as the old api for each interface. More on this in the "Multiple devices" section below.
- For compatibility reasons if multiple devices are given `net_now.carrier` and `net_now.state` correspond to the last interface in the iface array and should not be relied upon (deprecated).

### output

A textbox.

---

### Setting `iface` manually

If the widget [spawns a "no carrier" notification and you are sure to have an active network device](https://github.com/copycat-killer/lain/issues/102), then autodetection is probably not working. In this case you can set `iface` manually. You can see which device is **UP** with the following command:

```shell
ip link show
```

### Multiple devices

Since May 2016 `iface` can now be an array of the form `{ "eth0", "eth1", ... }` containing a list of the devices to collect data on.

If more than one device is included, `net_now.sent` and `net_now.received` will contain cumulative values over all given devices.
Use `net_now.devices["eth0"]` to access `sent`, `received`, `state` or `carrier` per device.

### Two widgets for upload/download rates from the same `iface`

[Read here](https://github.com/copycat-killer/lain/issues/61).