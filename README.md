# ESP8266_new_pwm
This is a drop-in replacement for the ESP8266 SDK PWM

The software PWM provided in the ESP8266 SDK from Espressif has several drawbacks:

1. Duty cycle limited to 90% (at 1kHz PWM period)
2. usable PWM period at most ~2KHz.
3. Incomplete documentation
 
This replacement allows duty cycles from 0% to 100%, with a stepsize of 200ns. This is 5000 steps for a 1kHz PWM, and 256 steps (8 bit of resolution) at 19kHz.

If all channels are in steady state (either 0% of 100% in any combination), the implementation goes to full idle, e.g. no interrupts.

The code is a drop-in replacement for the SDK, it provides the same functions as the SDK libpwm.a closed binary library. Just add pwm.c to your project.

By default there is one small difference to the SDK. The code uses a unit of 200ns for both period and duty. E.g. for 10% duty cycle at 1kHz you need to specify a period value of 5000 and a duty cycle value of 500, a duty cycle of 5000 or above switches the channel to full on.

To have full compatibility with the SDK, you have to set the SDK_PWM_PERIOD_COMPAT_MODE define to 1. If set, the code will use 1us for PWM period and 40ns for the duty cycle. E.g. 10% duty cycle at 1kHz is set by a period value of 1000 and a duty cycle value of 2500, full duty at 25000 and above.
