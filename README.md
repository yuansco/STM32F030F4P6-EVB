# STM32F030F4P6-EVB

This STM32 evaluation board added a built-in USB to UART IC PL2303SA to download code without additional components. 

<img src="https://github.com/yuansco/STM32F030F4P6-EVB/blob/master/Image/image3.PNG" style="width:550px;"/>


### Zephyr OS support

The existing stm32f030_demo is well compatible with this development board, we only need to modify some hardware differences in DTS:

1. Redefine led as GPIOB1
2. Modify the external crystal oscillator to 16MHz
3. Set PLLMUL to 3

```dts
--- a/boards/arm/stm32f030_demo/stm32f030_demo.dts
+++ b/boards/arm/stm32f030_demo/stm32f030_demo.dts
@@ -22,7 +22,7 @@
        leds {
                compatible = "gpio-leds";
                led: led {
-                       gpios = <&gpioa 4 GPIO_ACTIVE_HIGH>;
+                       gpios = <&gpiob 1 GPIO_ACTIVE_HIGH>;
                        label = "User LED";
                };
        };
@@ -34,13 +34,13 @@
 };
 
 &clk_hse {
-       clock-frequency = <DT_FREQ_M(8)>;
+       clock-frequency = <DT_FREQ_M(16)>;
        status = "okay";
 };
 
 &pll {
        prediv = <1>;
-       mul = <6>;
+       mul = <3>;
        clocks = <&clk_hse>;
        status = "okay";
 };
 ```

building blinky example
```
BOARD=stm32f030_demo
west build -p always -b $BOARD samples/basic/blinky
```

