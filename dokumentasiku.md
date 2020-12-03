# Dokumentasiku 

A. error ICCP incorrect sRGB profile [godot]:
  1. buka assets
  2. buka terminal
  3. ketik:
  `find . -name '*.png' -exec mogrify {} \;`

B. dd-ing images
  `dd bs=4M if=/path/to/archlinux.iso of=/dev/sdx status=progress oflag=sync`

C. restoring dd-ed storage
  `wipefs --all /dev/sdx`
  
D. Virtualbox efi shell 
  ```
  $ sudo mount /dev/sda1 /mnt
  $ cd /mnt
  $ sudo sh -c "echo '\EFI\ubuntu\grubx64.efi' > startup.nsh"
  ```

E. CARI DAFTAR FILES YG DIMILIKI PAKET
  1. debian: `dpkg -L namapaket`
  2. arch: `pacman -Ql namapaket`

F. RE-BUILD ISO BLANKON
```
ISOOPT="-v -A BlankOnCDFactory -p BlankOn -publisher BlankOn -V blankon"
ISOOPT="$ISOOPT -no-emul-boot -boot-load-size 4 -boot-info-table"
ISOOPT="$ISOOPT -R -b boot/grub/eltorito.img"
sudo genisoimage ${ISOOPT} -o uluwatu-desktop-amd64.iso -J -l -cache-inodes -allow-multidot extracted/
```

G. DOWNLOAD RECURSIVE WEB
```
wget \
     --recursive \
     --no-clobber \
     --page-requisites \
     --html-extension \
     --convert-links \
     --restrict-file-names=windows \
     --no-parent \
         alamat-website
```
         
H. SETTING UP DOCKER WITH FORWARDED IP
  1. `docker run -itp porthost:portdocker image:tag` 
  2. kalau sudah siap, tapi ada error `"run docker with --privileged"`  
  a. keluar lingkungan docker  
  b. commit pakai: `docker commit -m "pesan komit" -a "nama author" sha_container nama_image`  
  c. jalankan kembali image tadi pakai `docker run -itp porthost:portdocker --privileged image:tag` 17284,17285

I. CARI NAMA PAKET YG MEMILIKI FILE.INI
  1. debian: `dpkg -S namafile`
  2. arch: `pacman -Qo namafile`

J. SETTING MARIADB - gak bisa masuk mesti pakai sudo myqsl:  
  1. `sudo mysql_secure_installation`  
  2. `sudo mysql`  
  3. `UPDATE mysql.user SET plugin = 'mysql_native_password' WHERE user = 'root' AND plugin = 'unix_socket'; FLUSH PRIVILEGES;`
  4. `exit`
  5. silakan login kembali, sekarang pakai password ya!
 
K. SETTING VHOST UNTUK APACHE -> DOCKER
  ```
  <VirtualHost *:80>
    ServerName namadnsserver
    <Proxy *>
      Allow from localhost
    </Proxy>
    # http
    ProxyPass / http://localhost:8000/
  </VirtualHost>
  ```
L. CODEIGNITER SESSION DIR ERROR (MKDIR())
  ```
  application/config/config.php
  $config['sess_save_path'] = sys_get_temp_dir();
  ```
M. GIT STASH - SIMPAN PERUBAHAN TANPA COMMIT  
  1. membuat stash: `git stash create "pesan WIP"`
  2. lihat semua stash: `git stash list`
  3. mengembalikan pekerjaan WIP ke area kerja:  
  `git stash apply --index nomorindex`
  4. hapus stash:  
  `git stash drop nomorindex`
  ```
  
N. How to use Screen
`screen -S "kasih judul"` untuk buat baru
tekan `Ctrl + A` lalu `D` untuk detach
`screen -R` untuk reattach
