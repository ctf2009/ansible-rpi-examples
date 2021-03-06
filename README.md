## Ansible RPi Examples

This is a repository contaning a number of useful roles and example simple playbooks / scripts that can be installed on a Raspberry Pi

The following roles are currently included

- ping
- autohotspot (Based upon the tutorial at [http://www.raspberryconnect.com]([http://www.raspberryconnect.com)
- samba

#### Auto Hotspot

This role will create an auto hotspot as described at 

[http://www.raspberryconnect.com/network/item/331-raspberry-pi-auto-wifi-hotspot-switch-no-internet-routing](http://www.raspberryconnect.com/network/item/331-raspberry-pi-auto-wifi-hotspot-switch-no-internet-routing)

All credit for the auto hotspot scripts go to raspberryconnect.com

This is simply a demonstration of how to automate this for simple deployment onto a raspberry pi

This has been tested on Raspbian Stretch Lite April 2019

#### Samba

Currrenly this simply installs samba and supporting libraries. A future commit will add additional functionality




## Prerequisites 
This guide assumes the following. 

- You are running this from a *nix based OS
- You have Ansible installed: [https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- You have a freshly minted Raspberry Pi Raspbian SD card ready to go
- Your Raspberry Pi is running and is accessible from the local machine and has SSH enabled

If you have changed the hostname or the default user (pi) you will have to update the inventory file to reflect this

If you do not have any network to connect the Pi to, you can put the Pi into RNDIS / Gadget mode and connect directly via USB to run the commands. Very brief information in described here [http://www.circuitbasics.com/raspberry-pi-zero-ethernet-gadget/](http://www.circuitbasics.com/raspberry-pi-zero-ethernet-gadget/) 

**NOTE:** You may have to perform additional steps to allow the Pi to share the Internet connection with the host. This varies between OS


### How to Run

- Clone this repository to your local machine

- Run one of the scripts ... for example, `run_ping.sh` script to verify that the Pi is available. 

```
./run_ping.sh 
SSH password: 

PLAY [all] *************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************
ok: [raspberrypi.local]

TASK [ping : ping] *****************************************************************************************************
ok: [raspberrypi.local]

PLAY RECAP *************************************************************************************************************
raspberrypi.local          : ok=2    changed=0    unreachable=0    failed=0 

```
- An example of running the `autohotspot.sh` script

```
./run_provision.sh 
SSH password: 

PLAY [all] ******************************************************************************************************************************************************************************

TASK [Gathering Facts] ******************************************************************************************************************************************************************
ok: [raspberrypi.local]

TASK [autohotspot : create required directories] ****************************************************************************************************************************************
changed: [raspberrypi.local] => (item=/home/pi/autohotspot)

TASK [autohotspot : apt install libs] ***************************************************************************************************************************************************
[DEPRECATION WARNING]: Invoking "apt" only once while using a loop via squash_actions is deprecated. Instead of using a loop to supply multiple items and specifying `name: {{item}}`, 
please use `name: [u'hostapd', u'dnsmasq']` and remove the loop. This feature will be removed in version 2.11. Deprecation warnings can be disabled by setting 
deprecation_warnings=False in ansible.cfg.
changed: [raspberrypi.local] => (item=[u'hostapd', u'dnsmasq'])

TASK [autohotspot : stop services from running] *****************************************************************************************************************************************
changed: [raspberrypi.local] => (item=hostapd)
changed: [raspberrypi.local] => (item=dnsmasq)

TASK [autohotspot : sysctl] *************************************************************************************************************************************************************
changed: [raspberrypi.local]

TASK [autohotspot : hostapd configuration] **********************************************************************************************************************************************
changed: [raspberrypi.local]

TASK [autohotspot : hostapd defaults] ***************************************************************************************************************************************************
changed: [raspberrypi.local]

TASK [autohotspot : dnsmasq config] *****************************************************************************************************************************************************
changed: [raspberrypi.local]

TASK [autohotspot : copy example wpa_supplicant] ****************************************************************************************************************************************
changed: [raspberrypi.local]

TASK [autohotspot : umask hostapd] ******************************************************************************************************************************************************
changed: [raspberrypi.local]

TASK [autohotspot : lineinfile] *********************************************************************************************************************************************************
changed: [raspberrypi.local]

TASK [autohotspot : autohotspot script] *************************************************************************************************************************************************
changed: [raspberrypi.local]

TASK [autohotspot : autohotspot service file] *******************************************************************************************************************************************
changed: [raspberrypi.local]

TASK [autohotspot : autohotspot service] ************************************************************************************************************************************************
changed: [raspberrypi.local]

TASK [autohotspot : scripts] ************************************************************************************************************************************************************
changed: [raspberrypi.local] => (item=disable.sh)
changed: [raspberrypi.local] => (item=enable.sh)
changed: [raspberrypi.local] => (item=check.sh)

RUNNING HANDLER [autohotspot : reload systemd] ******************************************************************************************************************************************
changed: [raspberrypi.local]

PLAY RECAP ******************************************************************************************************************************************************************************
raspberrypi.local          : ok=16   changed=15   unreachable=0    failed=0 
```

Once completed, the auto hotspot should be enabled by default. If on restart a known Wifi connection cannot be found, the Pi will create its own Hotspot which defaults to ssid `raspberrypi` and password `raspberry`

You can even provide your own ssid and password as role variables if desired in the playbook. You can use Ansible Vault to protect the password

- You can run a playbook containing all the roles by running `run_all.sh` ... Currently this will ping the raspberry pi, install samba and deploy the autohotspot in one go. This will take the longest to run






