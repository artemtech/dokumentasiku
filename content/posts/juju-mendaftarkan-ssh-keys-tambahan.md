---
title: "Juju - Mendaftarkan SSH Pubkey Tambahan"
date: 2022-12-21T15:22:38+07:00
categories:
  - cloud
tags:
  - juju
  - ssh
---

Yang kita tahu, setelan bawaan Juju ketika pertama kali dipasang, akan membuat 1 pasangan kunci ssh di dalam folder (`$HOMEDIR/.local/share/juju/ssh/`),
dan juga kunci default user saat ini. Kunci inilah yang nantinya akan didaftarkan pada mesin-mesin atau unit-unit aplikasi pada berkas `.ssh/authorized_keys`.
Untuk mendaftarkan kunci lain yang biasa digunakan, kita bisa menggunakan perintah berikut:
```bash
juju add-ssh-key -m NAMA_MODEL "PASTE SSH KEY DISINI"
```

Setelah itu, coba lakukan ssh dengan pasangan kunci privat yang barusan telah didaftarkan kunci publiknya.

**notes**
- Mengapa kita gunakan -m NAMA_MODEL ? ya, karena setiap model memiliki entri ssh-keys yang berbeda, jika tidak mencantumkan -m NAMA_MODEL,
  maka secara otomatis Juju akan menambahkan kunci publik ssh itu ke dalam model yang saat ini sedang aktif.

Selamat mencoba !
