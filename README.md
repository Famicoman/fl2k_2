# FL2K_2 : a fork of the osmo_fl2K project.

Turns FL2000-based USB 3.0 to VGA adapters into low cost DACs.

For more information on source, see https://osmocom.org/projects/osmo-fl2k/wiki

This project fork is primarily for use with playing TBC files, but it can also be used as a replacement for the `osmos-fl2k` project.

## FL2K TBC Player

A Simple TBC playback utility, currently only CLI (Command Line Interface)

This will later be both GUI/CLI.

## What is a TBC filr?

A TBC (.tbc) file is a digital _Time Base Corrected_, lossless, 16-bit video file. Typically, one file is used for composite video streams and two files are used for s-video streams.

## How do I get a TBC file?

Via [VHS-Decode](https://github.com/oyvindln/vhs-decode) (Tape Decoding) and [LD-Decode](https://github.com/happycube/ld-decode) (LaserDisc Decoding) or [CVBS-Decode](https://github.com/oyvindln/vhs-decode/wiki/CVBS-Composite-Decode) (Composite Decoding).

You can also generate a TBC file from normal video using [ld-chroma-encoder](https://github.com/happycube/ld-decode/wiki/ld-chroma-encoder).

## Where you can buy the FL2K and adapters

The FL2K [Link 1](https://www.aliexpress.com/item/1005002872152601.html?) / [Link 2](https://www.reichelt.de/de/de/adapterkabel-usb-3-0-stecker-vga-buchse-schwarz-delock-62738-p287335.html)

VGA to RCA [Aliexpress](https://www.aliexpress.com/item/1005002872152601.html?)

VGA to BNC Male/Female [Amazon UK](https://www.amazon.co.uk/gp/product/B0033AF5Y0/) / [Amazon USA](https://www.amazon.com/s?k=VGA+to+BNC+Cable&crid=30JGI1TOFQ5I9&sprefix=vga+to+bnc+cable%2Caps%2C165&ref=nb_sb_noss_1)

## Hardware Setup

### Standardized Cable Configuration

#### Composite

Red - Right Audio

Blue - Left Audio

Green - Composite Video

#### S-Video

Green - Lumanace Y

Blue - Chroma C

Red - Mono/Mono Mix Audio

## Software Setup

### Windows

Download and install the stock driver [FL2000 Stock Driver](https://github.com/vrunk11/fl2k_2/fl2k_2/resources/FL2000-Driver-2.1.33676.0.exe)

Then select and replace the driver with libusb-win32 (v1.2.6.0) using [Zagig Driver Tool](https://github.com/vrunk11/fl2k_2/fl2k_2/resources/zadig-2.7.exe)

Simply download the latest [Windows Release](https://github.com/vrunk11/fl2k_2/releases).

Decompress the .zip file.

For GUI users

Open the fl2k_2.bat file.

For CLI users

Open an CMD Window and then open the directory your files are in, copy the path and add cd to the start example:

    cd C:\Users\harry\Desktop\fl2k

Once inside use arguments as stated below example:

    fl2k-2.exe -u -s pal -G16 -tbcG -G example.tbc


### Linux

*NOTE: The Linux version currently does not work as expected. Follow the below at your own risk.*

The instructions below assume a non-root, `sudo`-capable user on a Debian-based distribution.

first we need to download dependencies:

```
sudo apt update && sudo apt install libusb-1.0-0-dev libsoxr-dev libsoxr0 libsoxr-lsr0 git
```

We can install the original `osmo-fl2k` tools using these steps:

```
git clone https://gitea.osmocom.org/sdr/osmo-fl2k
mkdir osmo-fl2k/build
cd osmo-fl2k/build
cmake ../ -DINSTALL_UDEV_RULES=ON
make -j 3
sudo make install
sudo ldconfig
```

Before being able to use the device as a non-root user, the udev rules need to be reloaded:

```
sudo udevadm control -R && sudo udevadm trigger
```

To install the TBC player:

```
git clone https://github.com/vrunk11/fl2k_2.git fl2k-tbc-player
cd fl2k-tbc-player
wget -P include/ https://raw.githubusercontent.com/chirlu/soxr/refs/heads/master/src/soxr.h
chmox +x compile.sh
./compile.sh
```

## Usage

Only the __Red__ lead is supported for video output.

### Composite output on the red channel:

Linux:

```
fl2k_file2 -s ntsc -R16 -tbcR -R example-decode.tbc
```

Windows:

```
fl2k_file2.exe -s ntsc -R16 -tbcR -R example-decode.tbc
```

### S-Video output with luma on the green channel and chroma on the blue channel:

Linux:

`fl2k_file2 -u -s pal -G16 -tbcG -G example.tbc -B16 -tbcB -B example_chroma.tbc`

Windows:

`fl2k_2.exe -u -s pal -G16 -tbcG -G example.tbc -B16 -tbcB -B example_chroma.tbc`

### Arguments

```
[-d device_index (default: 0)]
[-s samplerate (default: 100 MS/s) you can write(ntsc) or (pal)]
[-u Set the output sample type of the fl2K to unsigned]
[-R filename (use '-' to read from stdin)
[-G filename (use '-' to read from stdin)
[-B filename (use '-' to read from stdin)
[-A audio file (use '-' to read from stdin)
[-syncA chanel used for sync the audio file  default : G  value = (R ,G ,B)
[-R2 secondary file to be combined with R (use '-' to read from stdin)
[-G2 secondary file to be combined with G (use '-' to read from stdin)
[-B2 secondary file to be combined with B (use '-' to read from stdin)
[-R16 (convert bits 16 to 8)
[-G16 (convert bits 16 to 8)
[-B16 (convert bits 16 to 8)
[-R8 interpret R input as 8 bit
[-G8 interpret G input as 8 bit
[-B8 interpret B input as 8 bit
[-resample active output resampling
[-signR interpret R input as (1 = signed / 0 = unsigned) or (s = signed / u = unsigned)
[-signG interpret G input as (1 = signed / 0 = unsigned) or (s = signed / u = unsigned)
[-signB interpret B input as (1 = signed / 0 = unsigned) or (s = signed / u = unsigned)
[-cmbModeR combine mode  default : 0  value = (0 ,1 ,2)
[-cmbModeG combine mode  default : 0  value = (0 ,1 ,2)
[-cmbModeB combine mode  default : 0  value = (0 ,1 ,2)
[-tbcR interpret R as tbc file
[-tbcG interpret G as tbc file
[-tbcB interpret B as tbc file
[-not_tbcR disable tbc processing for input R file
[-not_tbcG disable tbc processing for input G file
[-not_tbcB disable tbc processing for input B file
[-CgainR chroma gain for input R (0.0 to 6.0) (using color burst)
[-CgainG chroma gain for input G (0.0 to 6.0) (using color burst)
[-CgainB chroma gain for input B (0.0 to 6.0) (using color burst)
[-SgainR signal gain for output R (0.5 to 2.0) (clipping white)
[-SgainG signal gain for output G (0.5 to 2.0) (clipping white)
[-SgainB signal gain for output B (0.5 to 2.0) (clipping white)
[-VmaxR maximum output voltage for channel R (0.003 to 0.7) (scale value) (disable Cgain and Sgain)
[-VmaxG maximum output voltage for channel G (0.003 to 0.7) (scale value) (disable Cgain and Sgain)
[-VmaxB maximum output voltage for channel B (0.003 to 0.7) (scale value) (disable Cgain and Sgain)
[-MaxValueR max value for channel R (1 to 255) (reference level) (used for Vmax)
[-MaxValueG max value for channel G (1 to 255) (reference level) (used for Vmax)
[-MaxValueB max value for channel B (1 to 255) (reference level) (used for Vmax)
[-ireR IRE level for input R (-50.0 to +50.0)
[-ireG IRE level for input G (-50.0 to +50.0)
[-ireB IRE level for input B (-50.0 to +50.0)
[-FstartR seek to frame for input R
[-FstartG seek to frame for input G
[-FstartB seek to frame for input B
[-audioOffset offset audio from a duration of x frame
[-pipeMode (default = A) option : A = Audio file / R = output of R / G = output of G / B = output of B
[-readMode (default = 0) option : 0 = multit-threading (RGB) / 1 = hybrid (R --> GB) / 2 = hybrid (RG --> B) / 3 = sequential (R -> G -> B)
```

### Possible USB Issues

You might see this in Linux when starting `fl2k_file2`:

```
Allocating 6 zero-copy buffers
libusb: error [op_dev_mem_alloc] alloc dev mem failed errno 12
Failed to allocate zero-copy buffer for transfer 4
```

If so you can increase your allowed `usbfs` buffer size with the following command (as `root` only) and reboot:

```
su -
echo 0 > /sys/module/usbcore/parameters/usbfs_memory_mb
reboot now
```

This will result in better I/O stability and reduced CPU usage. This config was added to the kernel [back in 2014](https://lkml.org/lkml/2014/7/2/377). The default buffer size is 16.

## Attribution

Based off the [osmo_fl2K project](https://osmocom.org/projects/osmo-fl2k/wiki) software.
