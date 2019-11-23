# 🟥🟩🟦 Project Lux

**Project Lux** is a collection of components for driving
WS2812B RGB LED strips over networks.

## Software Components
* **luxsrv** - Lua program for the NodeMCU ESP2866 devkit that listens for 
network packets describing the color setup and driving the WS2812B LED strip;
* **luxaudio** - Go program that captures audio, does spectral analysis on it, and sends 
a spectrum visualization to the strip over the network;
* **luxfx** - TypeScript application with a few predefined effects. It can work either
with a locally-connected strip (connected to the same device running the program), 
or following the protocol to drive network-attached strips.

## Network Protocol
The **luxsrv** component listens for UDP packets on port **`42170`**.


### Generic Packet Structure
UDP packets that **luxsrv** understands have the following generic structure:
```
|    2    |       1     |        N       |
|  Header | Effect Mode | Effect Payload |
| x4C x58 |    x00-01   |       ...      |
```


### Raw Mode (`0x00`)
In raw mode, clients send the number of LED pixels to drive (up to 255),
followed by a sequence of GRB components giving the color for each of the pixels.

```
|    2    |       1     |        1        |              (PixelCount * 3)           | 
|  Header |   Raw Mode  | LED Pixel Count |                 GRB data                |
| x4C x58 |      x00    |      x00-FF     | px0g, px0r, px0b, px1g, px1r, px1b, ... |
```

For example, to light up the 1st pixel in red, the 2nd one in green, and the 3rd in blue:
```js
[ 
    0x4C, 0x58,        // header 
    0x00,              // mode = raw
    0x03,              // pixel count = 3 
    0x00, 0xFF, 0x00,  // 1st pixel GRB color = 0x00FF00 (red)
    0xFF, 0x00, 0x00,  // 2nd pixel GRB color = 0xFF0000 (green)
    0x00, 0x00, 0xFF,  // 3rd pixel GRB color = 0x0000FF (blue)
]
```

## Hardware Components
* **WS2812B RGB LED Strip** - [Buy on AliExpress](https://www.aliexpress.com/item/2036819167.html)
* **NodeMCU ESP2866 DevKit** - [Buy on AliExpress](https://www.aliexpress.com/item/32656775273.html)
