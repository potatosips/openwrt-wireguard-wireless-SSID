Assuming you have fresh installed openwrt with sysupgrade done
also have a working wireguard peer config

ssh into router
opkg update
opkg list-upgradable | cut -f 1 -d ' ' | xargs -r opkg upgrade 
opkg install luci-app-wireguard
reboot

goto network -> interface -> add interface
name wg
protocol wireguard vpn
device unspecified
create interface
check bring up on boot
click load configuration
paste the wireguard config in the box
it should look like this
![image](https://github.com/potatosips/openwrt/assets/118026260/a79cec96-061c-447c-bfea-63b93c5cbb91)
-> goto advanced settings
remove Use default gateway
-> goto firewall settings
click the dialogue box -> write wg -> hit enter
-> goto peer -> edit 
check Route Allowed IPs
save 
save & apply
reboot router

goto network -> interface -> Devices (tab) -> add device configuration
device type bridge device
device name br-wg
check bring up empty bridge 
save
save & apply
it should look like this
![image](https://github.com/potatosips/openwrt/assets/118026260/fb2db1f0-28de-4737-a0ce-f8448814bde9)


goto network -> interface -> interfaces (tab) -> add interface
name wg_lan
protocol static address
device br-wg
check bring up on boot
ipv4 address 192.168.2.1
ipv4 mask 255.255.255.0
it should look like this
![image](https://github.com/potatosips/openwrt/assets/118026260/28021e59-f0e4-417c-b156-c1d958af36e6)
-> goto advanced settings
remove Use default gateway
-> goto firewall settings
click the dialogue box -> write wg_lan -> hit enter
-> goto DHCP server 
enable DHCP
save
save & apply

goto network -> firewall (zones)
edit wg firewall zone like this
![image](https://github.com/potatosips/openwrt/assets/118026260/0fab83b5-5c0b-4c3d-a422-234d42cf6618)
then edit wg_lan firewall zone like this
![image](https://github.com/potatosips/openwrt/assets/118026260/ae57eed2-ba39-4065-97d8-f7c00d42cb2a)
save
save & apply

goto network -> firewall -> Traffic Rules
add a traffic rule
![image](https://github.com/potatosips/openwrt/assets/118026260/83447302-ae1a-4282-9bf2-1d5d80736088)
![image](https://github.com/potatosips/openwrt/assets/118026260/53b8907e-5e25-4e06-9522-46ad90a7eb80)


goto network -> Routing -> Static ipv4 Routes
add like this
change gateway to wireguard server's gateway
![image](https://github.com/potatosips/openwrt/assets/118026260/2da329bd-6a5f-4462-b642-39d32722f0ad)

goto network -> Routing -> ipv4 Rules
![image](https://github.com/potatosips/openwrt/assets/118026260/72d4df10-506a-442d-b202-256b0284bc00)

goto network -> Wireless
create or edit a wireless interface
just change the network like this
![image](https://github.com/potatosips/openwrt/assets/118026260/fef0517e-431a-45ca-918b-529e6cd1a0c2)







