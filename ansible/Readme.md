# Ansible to configure RPI

## overall

* used ubuntu 20.04 64bit or raspberry 64bit
* gpio seems not to work on ubuntu 20.04 64 bit (iomem=relaxed in cmdline.txt doesn't help)
* gpio deprecated on actual RPI images
* use FDTI (important) USB serial device (Amazon: CableCreation USB auf RS232 Seriell Kabel, 1M Vergoldet USB 2.0 zu RS232 Stecker DB9 Seriell Konverter Kabel mit FTDI Chipsatz für Kasse Register Modem Scanner Industriual Maschinen CNC usw. Schwarz)
* other cable without fdti seems not to work (driver issue)
* FTDI works on linux, mac and Windows
* on linux you need to diasble other libs (rmmod ftdi_sio, add ftdi_sio to /etc/modprobe.d/blacklist.conf)

login problems
<https://wiki.archlinux.org/title/LightDM#Enabling_autologin>

```bash
2020-08-02 13:36:25 initPeripherals: mmap gpio failed (Operation not permitted)
pigpio initialisation failed.
```

## prepare RPI

### Example Raspberry

* download 64bit raspberry (Raspberyy pi iamger)
* add ssh file at /boot partition (touch ssh)
* start from rpi

### Passwordless

Generate keys and copy key to raspberry

```bash
ssl-keygen 
# copy pub file to authorized_keys
```

* inventoiry/group_vrs/all.yml - add domain name
* inventory/hosts - dd server in hosts with ssh key file

### some base configs for Raspberry

* first configure with attached monitor, mouse and keyboard
* be sure password was set in GUI
  * set language settings
  * set hostname
* remove all standby/powersave features
* enable in raspi-config
  * enable vnc
  * VNC - set VNC resolution for headless 1080P
  * enable/check ssh
* connect with vnc as user pi and do configs
* prepare ssh connect (passwordless - default user pi/raspberry)
* deploy ansible scripts  
  
## Install

### normal Install

Special global_clean_all=true (for first time install)

* reinstall npm
* reboot on common install (remove all services)
* generate new cert nginx (frontendservices)
* clean directories

```bash
# basic components - packages npm etc.
# always
ansible-playbook -i inventories/production/hosts common.yml --limit=<server of hosts>  -e global_clean_all=true

# backend small (without cassandra)
# -e mapper_cloud=true (add service for cloud)
ansible-playbook -i inventories/production/hosts backendServices.yml --limit=<server of hosts>  -e global_clean_all=true

# backend large add cassandra more than 2GB needed
ansible-playbook -i inventories/production/hosts storeBackend.yml --limit=<server of hosts>  -e global_clean_all=true

# frontends
ansible-playbook -i inventories/production/hosts frontendServices.yml --limit=<server of hosts>  -e global_clean_all=true

```

ansible-playbook site.yml -i inventories/production/hosts --limit vmbox
ansible-playbook -i inventories/production/hosts testService.yml --limit=rpi 

### overwrite globals

ansible-playbook cassandra.yml -i inventories/production/hosts --limit vmbox -e global_clean_all=true

## Other modules

### webcam

[Webcam](roles/webcam/Readme.md)
