netsh advfirewall firewall add rule name="ICMP Block" dir=in  action=block protocol=icmp
dir=
in -- qurilmaga keladigan 
out qurilmadan chiqadigan 

action=
block -- paketlarni o'tkazmaslik
allow -- keladigan paketlarga ruxsat berish
netsh advfirewall firewall add rule name="BLOCK_HTTPS_OUT" dir=out action=block protocol=tcp remoteport=443 enable=yes
netsh advfirewall firewall add rule name="BLOCK_HTTPS_IN" dir=in action=block protocol=tcp localport=443 enable=yes
Tizimda butun boshlik internetni uzish
Servislar haqida
tasklist
sc
pslist
net start -- bsohlash
net stop -- toxtatish