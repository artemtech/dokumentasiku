---
title: "Postgresql Failover dengan 2 Node"
draft: true
date: 2023-02-07T02:30:38+07:00
categories:
  - cloud
tags:
  - postgresql
  - failover
  - repmgr
---

Hoamm, ngantuk wkwk.  

Beberapa waktu lalu dapat challange, gimana caranya deploy HA postgresql tapi nodenya terbatas hanya 2 node. Dimana-mana HA minimal 3 node kan?
Apalagi untuk service database, masa cuma 2 sih? Ya namanya aja challange, ya mesti sulapan :D

Banyak toolsnya sih sebenernya. Beberapa diantaranya: symmetricds, repmgr, patroni, dan skrip rakitan sendiri :D. Yang kasih challange kemaren menyarankan 
untuk pakai symmetricds. Tapi saya ogah pakainya, karena java-based 😂

Kemudian ada saran dari temen kantor, pakai patroni aja buat HA nya. Nanti dokumentasi patroninya saya tulis terpisah, terlalu panjang disini, ehehe. Nah setelah coba patroni,
bisa tuh jalan HA nya, tapi... Patroni minimal 3 node, sama seperti galera di MariaDB. Udah coba paksain pakai patroni di 2 node dengan mengakali etcd-nya tapi tetep aja, satu node mati
ambyar failover-nya.

<bersambung>
