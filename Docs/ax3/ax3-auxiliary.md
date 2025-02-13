# AX3/AX6 Auxiliary Data: Battery, Temperature, Light

## Auxiliary Data

The AX3/AX6 device primarily records high-frequency movement data, but also record additional, low-frequency, *auxiliary* data.  This data includes light level indication, device temperature, and device battery voltage.  This data is always recorded, regardless of configuration, whenever the device is logging movement data.

The sensors are internally sampled at 1 Hz, then the most recently sampled value is only stored once per written data "sector" -- this is every 120 samples (AX3 packed mode), 80 samples (AX3 unpacked mode, or AX6 in accelerometer-only mode), or 40 samples (AX6 with gyroscope enabled).

See below for specific information on the battery, temperature, and light.


## Battery Voltage

The device battery voltage is sampled as a 10-bit ADC value (0-1023).  

<!--
Only the top half of this range is useful, and the value is stored into a 8-bit range of 0-255 as: ${packed = \lfloor(value - 512) / 2\rfloor}$; and restored on reading as: ${value = packed * 2 + 512}$.  
-->

To convert the battery ADC values into voltage (Volts), the conversion is:

$$
voltage = value * 6 / 1024
$$


## Device Temperature

The device internal temperature is measured by an on-board temperature sensor ([MCP9700](https://www.microchip.com/en-us/product/MCP9700)) and is sampled and stored as a 10-bit ADC value (0-1023).  

To convert the temperature ADC values into degrees Celsius, the conversion is:

$$
temperature = (value * 75 / 256) - 50
$$

<!--
The internal temperature sensor is useful for auto-calibration of the movement data.
-->


## Light Level Indicator

The AX devices use a light sensor ([APDS-9007](https://docs.broadcom.com/docs/AV02-0512EN)) which has a logarithmic response over a wide dynamic range.  The sensor arrangement is most suitable as a general, relative, indicator of light, for example to distinguish a varying/stable level, or daily maxima/minima. 

The AX light level indicator is complicated as, in order to not compromise the enclosure's protection, the sensor is used without a full optical window, and so its view is through the (partially transparent) case material and, if used, (partially transparent) strap.  Sensors worn on the wrist might become obscured in use by the wearer's clothing or bedclothes, and subject to reflections, shadows, etc.  

The logarithmic output means that light is detectable through the enclosure or strap. For example, changes from 10-100 lux and 1 Klux - 10 Klux both have a relative change of *10* (which is a *10 uA* difference in the sensor output, and the AX3 uses a load resistor of 100 kOhm to convert this 10 uA change into a 1 V change, which becomes 341.3 raw 10-bit ADC units).

**AX6:** To make better use of the range, the AX6 uses a load resistor of only 10 kOhm (e.g. converts a 10 uA change into a 0.1 V change, which becomes 34.13 raw ADC units).  For AX6 data, multiply raw values by 10 to be equivalent to the AX3 raw values discussed below.

For many applications, it may be best to use the raw ADC values as a relative indicator of light, as it remains in the linear space of perception.  If you are certain that you want the Lux value, to convert the light ADC values into Lux, the conversion is:

$$
lux = 10^{(value + 512) * 6 / 1024}
$$

As the recorded level is logarithmic, even small measurement noise or inter-device variation will become large when converting to lux.  As lights tends towards the upper limit, the output curve of the sensor starts to become shallow in the kLux ranges, so digitized values may become increasingly noisy towards saturation.


## Exporting/Loading Auxiliary Data

### CSV Exporter

One way to quickly export the auxiliary data from the CWA file is to use the [`cwa-convert`](https://github.com/digitalinteraction/openmovement/tree/master/Software/AX3/cwa-convert/c) tool to create a .CSV file.  See link for cross-platform usage or, if you have [OmGui](https://github.com/digitalinteraction/openmovement/wiki/AX3-GUI) installed, you can run the command (where `CWA-DATA.CWA` is your filename):

```cmd
"%ProgramFiles(x86)%\Open Movement\OM GUI\Plugins\Convert_CWA\cwa-convert.exe" "CWA-DATA.CWA" -nodata -t:block -skip 240 -light -temp -batt > "OUTPUT.CSV"
```

The export will skip 240 samples, so not export quite every auxiliary value, but they are slow-changing.  The exported columns will be:

> `Time, LightADC, TemperatureADC, BatteryADC`

You can alternatively output the battery voltage with the `-battv` parameter, and the temperature in degrees Celsius with the `-tempc` parameter (the latter will only work on later versions of `cwa-convert`).

If you would like to export the data, along with (possibly duplicated) auxiliary data, remove the `-nodata`, `-t:block`, and `-skip` parameters:

```cmd
"%ProgramFiles(x86)%\Open Movement\OM GUI\Plugins\Convert_CWA\cwa-convert.exe" "CWA-DATA.CWA" -light -temp -batt > "OUTPUT.CSV"
```

The exported columns will be, for AX3 or AX6 accelerometer-only mode:

> `Time, Ax(g), Ay(g), Az(g), LightADC, TemperatureADC, BatteryADC`

...or, for AX6 with gyroscope enabled:

> `Time, Ax(g), Ay(g), Az(g), Gx(d/s), Gy(d/s), Gz(d/s), LightADC, TemperatureADC, BatteryADC`

<!--
Unlabelled data can usually be identified as the A columns will have a vector magnitude of 1 at rest, the G columns will have a vector magnitude of 0 at rest, the light reading will generally have cases of abrupt fluctuations, while the temperature reading will only change gradually.  Raw ADC values, rather than converted values, can be spotted as they are always whole numbers in the range 0-1023.
-->


### MATLAB Loader

The MATLAB loader [CWA_readFile.m](https://raw.githubusercontent.com/digitalinteraction/openmovement/master/Software/Analysis/Matlab/CWA_readFile.m) can be used to load the light (raw values) and temperature (Celsius):

```matlab
data = CWA_readFile('CWA-DATA.CWA', 'modality', [1 1 1]);
% data.AXES  %% (time Ax Ay Az) or (time Ax Ay Az Gx Gy Gz)
% data.LIGHT %% (raw light values)
% data.TEMP  %% (degrees Celsius)
```


### Python Loader

The Python loader [openmovement-python](https://github.com/digitalinteraction/openmovement-python#cwa_load---cwa-file-loader) can be used to load the light and temperature ADC values as follows:

```python
with CwaData(filename, include_gyro=False, include_light=True, include_temperature=True) as cwa_data:
    # As an ndarray of [time,accel_x,accel_y,accel_z,light,temperature]
    sample_values = cwa_data.get_sample_values()
    # As a pandas DataFrame
    samples = cwa_data.get_samples()
```

