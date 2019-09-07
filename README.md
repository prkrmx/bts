OpenBTS project on a fresh Ubuntu 16.04 server with uhd.

## Components
The installation consists next source builds:
- [UHD](http://uhd.ettus.com)
- [OpenBTS](https://github.com/RangeNetworks/dev)
- [Systemd Scripts](https://github.com/nadiia-kotelnikova/openbts_systemd_scripts)

Support B200mini and B205mini usrp devices :+1: 

Installation goes without errors if you do not install UHD driver manually before OpenBTS installation. For the Ettus manufacture build.sh script automatically installs UHD driver from the Ubuntu repository. I would suggest installing from the beginning on fresh OS. Here what I am doing:
## Configure and Build
```
sudo apt install git -y
git clone https://github.com/prkrmx/bts.git src
cd src/
./build.sh B205mini
cd BUILDS/20XX-XX-XX--XX-XX-XX/
sudo dpkg -i *.deb
```
After the installation process is complete, configure the services
```
cd 
sudo cp src/helpers/systemd/* /etc/systemd/system/
sudo systemctl reload
```
Load UHD images
```
sudo /usr/lib/uhd/utils/uhd_images_downloader.py
sudo uhd_usrp_probe
linux; GNU C++ version 5.3.1 20151219; Boost_105800; UHD_003.009.002-0-unknown

-- Loading firmware image: /usr/share/uhd/images/usrp_b200_fw.hex...
-- Detected Device: B205mini
-- Loading FPGA image: /usr/share/uhd/images/usrp_b205mini_fpga.bin... done
-- Operating over USB 3.
-- Initialize CODEC control...
-- Initialize Radio control...
-- Performing register loopback test... pass
-- Performing CODEC loopback test... pass
-- Asking for clock rate 16.000000 MHz...
-- Actually got clock rate 16.000000 MHz.
-- Performing timer loopback test... pass
-- Setting master clock rate selection to 'automatic'.
  _____________________________________________________
 /
|       Device: B-Series Device
|     _____________________________________________________
|    /
|   |       Mboard: B205mini
|   |   revision: 3
|   |   product: 30522
|   |   serial: 3180EA7
|   |   name: RX_1
|   |   FW Version: 8.0
|   |   FPGA Version: 4.0
|   |
|   |   Time sources: none, internal, external
|   |   Clock sources: internal, external
|   |   Sensors: ref_locked

```
Done!
## Start Stop
```
sudo src/helpers/openbts-start.sh
sudo src/helpers/openbts-stop.sh
```
## Setup
Receiver gain setting 0-10 dB for Ettus hardware.
```
/OpenBTS/OpenBTSCLI -c 'devconfig GSM.Radio.RxGain 10'
```
