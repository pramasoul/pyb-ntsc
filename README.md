# Analog TV with pyboard

An analog television station made from a pyboard and two external components.

## Hardware

![pyboard](https://store.micropython.org/media/products/PYBv1_1-B.jpg)
![pyboard](https://store.micropython.org/media/products/PYBv1_1-E.jpg)

1. A [pyboard](https://store.micropython.org/product/PYBv1.1). Tested with a version 1.0 board running at 168 MHz *__FIXME:__ Test on v1.1 board*
1. An N channel signal MOSFET. I used a [5LN01SP](https://cdn.sparkfun.com/assets/learn_tutorials/4/2/3/5ln01sp.PDF) from a [SparkFun Discrete Semiconductor Kit](https://www.sparkfun.com/products/13682) (component [identification guide](https://learn.sparkfun.com/tutorials/discrete-semiconductor-kit-identification-guide/all))
1. A small 1kΩ resistor.

### Pinouts
* DAC 1 (X5)  - outputs the video signal
* X1          - outputs the RF carrier

#### Diagram
```
   (X1)──┐
        ┌┴┐
        │1│ r = 1 k ohm
        │k│
        └┬┘
         └──┐
            ├─────────── to wire antenna
     gate┃┠─┘drain
   (X5)──┨┃
         ┃┠─┐source
            └─────┐
                 ─┴─
                 ╶─╴
                  ─
                 gnd
```

The FET acts as a voltage-variable resistive load to
 modulate the amplitude at the antenna node. A short length of
 wirewrap or magnet wire will suffice for single-room coverage.

![5LN01SP pinout](https://cdn.sparkfun.com/assets/learn_tutorials/4/2/3/5ln01sp-legend.png)

## Software

The magic numbers for blanking_level, black_level, and white_level
 were found experimentally. More greyscale range can be gotten by
 modifying the schematic or using a better suited mosfet mixer.

The pyboard's DAC is rated for 1 MHz operation, which corresponds to
 the default argument of hres=64 for the constructor.  NOTE: hres
 should be even if `progressive == False` (else you'll get a jittery
 picture)

On my pyboard, it worked up to `hres=124` interlaced but not above.
 After a code refactor, it works up to `hres=250` progressive scan, with higher resolutions
 inaccessible due to both lack of available memory and DAC slew rate limitation.

progressive=False results in ~ 30 fps interlaced with 482 lines vertical resolution.
progressive=True  results in ~ 60 fps with 241 lines vertical resolution.

### Demo
* mandelbrot set zoom
  * tilt pyboard to control cursor, press USR button to zoom in.
  * Hold USR button for > 1 second then release to double iterations.
  * Hold USR button for > 1 second, flip pyboard over, then release to
   toggle julia set mode.

  * It uses single precision floats so don't expect to be able to do deep zooms.




