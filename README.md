# Protocentral MAX30001 Zephyr out of tree driver

This is the Zephyr driver for the MAX30001 single-lead ECG / BioZ monitoring IC from Maxim.

MAX30001 is a single-lead ECG monitoring IC which has built-in R-R detection and several other features that make it perfect for a wearable single-lead ECG application.

## Usage

To use this driver in your Zephyr project, you need to add the following to your 'west.yml' file, under the 'projects' section:

```yaml
- name: MAX30001
      path: modules/protocentral_max30001
      revision: main
      url: https://github.com/Protocentral/protocentral_max30001_zephyr_driver
```

Example 'west.yml' file:

```yaml
manifest:
    self:
        west-commands: scripts/west-commands.yml

    remotes:
        - name: zephyrproject-rtos
        url-base: https://github.com/zephyrproject-rtos

    projects:
        - name: zephyr
            remote: zephyrproject-rtos
            revision: v3.5-branch
        - name: MAX30001
            path: modules/protocentral_max30001
            revision: main
            url: https://github.com/Protocentral/protocentral_max30001_zephyr_driver
```

You also need to add the following to your 'prj.conf' file:

```c
CONFIG_SENSOR=y
CONFIG_MAX30001=y
```

In the devicetree (DTS) file or DTS overlay, you need to add the following under the appropriate SPI node:

```
&spi3 {
	compatible = "nordic,nrf-spim";
	status = "okay";

	pinctrl-0 = <&spi3_default>;
	pinctrl-1 = <&spi3_sleep>;
	pinctrl-names = "default", "sleep";
	cs-gpios = <&gpio1 10 GPIO_ACTIVE_LOW>;

	max30001: max30001@0 {
		compatible = "maxim,max30001";
		reg = <0x0>;
		spi-max-frequency = <DT_FREQ_M(1)>;
		status = "okay";
	};
};
```

That's it - you can now use the MAX30001 driver in your Zephyr project.

## Compatibility

This module has been tested with Zephyr v3.5 and v3.6 on Nordic nRF52 and nRF53 series SoCs as well as on the Raspberry Pi Pico/RP2040.

It should work with other versions of Zephyr as well.

## Example

An example project that uses this driver will be added soon.
