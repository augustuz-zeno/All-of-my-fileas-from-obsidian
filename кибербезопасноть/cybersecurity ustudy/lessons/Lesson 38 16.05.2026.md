[[0 Ustudy]]
был повтор и пройден урок которого не было
Linuxda firewall orniga iptables ishlatilinadi.

iptables ing 3ta chain bor, yani 3xil turdagi qonun qoidasi bor, vazifasiga kora 3ga bolinadi.

1. Input -- qurilmamizga kelayotgan malumotlar uchun qonun biriktiramiz
2. Output -- qurilmamizdan chiqayotgan paketlar uchun qonun biriktiramiz
3. Forward -- togridan togri biz orqali otadigan malumot paketlar uchun qonun biriktiramiz.

Har bir chain(zanjir) da 3 xil turdagi qonunlar bor.

1. Accept -- kelayotgan paketlarni qabul qilish
2. Drop  -- Kelayotgan paketlarni shunchaki otqazib yuboradi va foydalanuvchiga xabar bermaydi
3. Reject  -- Kelayotgan paketlarni ignor qiladi va bu haqida xabar beriladi


sudo iptables -L -- tizimdagi iptablelarni royxatini chiqaradi.

Hamma zanjirlarda default tarzda Accept qoidasi kiritilgan boladi.

-n -- numeric formatda malumotlarni chiqaradi
-v -- verbose yani koproq malumotlar beradi

iptables optionslari:
-P -- Policy -- zanjirlarda qoidani ornatish uchun ish-di
-A -- APPEND -- qoshilayotgan qoida qaysi zanjir uchunligini bildirish
-s -- source ip -- paket jonatayotgan qurilma
-d -- destination ip -- paket qabul qiladigan qurilma
-I -- Insert -- yangi qoshilayotgan qonun qoidalar qaysi Orinda qoshilishini bildirradi
-j -- qoshilayogan yangi qoida qanaqa bolishini bildiradi(Accept, Reject, DROP)
-D -- delete -- qaysidir iptables ni ochirish
-R -- Replace -- qoshilgan iptables ni edit qilish mumkin


Servis turlari 2 xil:

1. SystemD -- target unit -- komanda - systemctl
komanda sintaksis -- sudo systemctl start ssh
2. SysV.Init -- runlevel -- komanda - service
komanda sintaksis -- sudo service ssh start