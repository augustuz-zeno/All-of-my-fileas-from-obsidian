[[0 Ustudy]]
Linuxda firewall orniga iptables ishlatilinadi.

iptables ing 3ta chain bor, yani 3xil turdagi qonun qoidasi bor

1. Input -- qurilmamizga kelayotgan malumotlar uchun qonun biriktiramiz
2. Output -- qurilmamizdan chiqayotgan paketlar uchun qonun biriktiramiz
3. Forward -- togridan togri biz orqali otadigan malumot paketlar uchun qonun biriktiramiz.

Har bir chain(zanjir) da 3 xil turdagi qonunlar bor.

1. Accept -- kelayotgan paketlarni qabul qilish
2. Drop  -- Kelayotgan paketlarni shunchaki otqazib yuboradi
3. Reject  -- Kelayotgan paketlarni ignor qiladi 


sudo iptables -L -- tizimdagi iptablelarni royxatini chiqaradi.

Hamma zanjirlarda default tarzda Accept qoidasi kiritilgan boladi.

-n -- numeric formatda malumotlarni chiqaradi
-v -- verbose yani koproq malumotlar beradi