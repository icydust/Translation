# FAQ

# Frequently Asked Questions

## Build Errors

### Flash Overflow

<aside class="tip">
Use the FMUv4 architecture to obtain double the flash. The first available board from this generation is the [Pixracer](http://dev.px4.io/hardware-pixracer.html).
</aside>

The amount of code that can be loaded onto a board is limited by the amount of flash memory it has. When adding additional modules or code its possible that the addition exceeds the flash memory. This will result in a "flash overflow". The upstream version will always build, but depending on what a developer adds it might overflow locally.

<div class="host-code"></div>

```sh
region `flash' overflowed by 12456 bytes
```

To remedy it, either use more recent hardware or remove modules from the build which are not essential to your use case. The configurations are stored [here](https://github.com/PX4/Firmware/tree/master/cmake/configs). To remove a module, just comment it out:

<div class="host-code"></div>

```cmake
#drivers/trone
```

## USB Errors

### The upload never succeeds

On Ubuntu, deinstall the modem manager:

```sh
sudo apt-get remove modemmanager
```