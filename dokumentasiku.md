A. error ICCP incorrect sRGB profile [godot]:
  1. buka assets
  2. buka terminal
  3. ketik:
  find . -name '*.png' -exec mogrify {} \;

B. dd-ing images
  dd bs=4M if=/path/to/archlinux.iso of=/dev/sdx status=progress oflag=sync

C. restoring dd-ed storage
  wipefs --all /dev/sdx
  
D. Virtualbox efi shell
  $ sudo mount /dev/sda1 /mnt
  $ cd /mnt
  $ sudo sh -c "echo '\EFI\ubuntu\grubx64.efi' > startup.nsh"

E. dpkg-query -L namapaket
F. RE-BUILD ISO BLANKON
```ISOOPT="-v -A BlankOnCDFactory -p BlankOn -publisher BlankOn -V blankon"
ISOOPT="$ISOOPT -no-emul-boot -boot-load-size 4 -boot-info-table"
ISOOPT="$ISOOPT -R -b boot/grub/eltorito.img"
sudo genisoimage ${ISOOPT} -o uluwatu-desktop-amd64.iso -J -l -cache-inodes -allow-multidot extracted/
```

G. DOWNLOAD RECURSIVE WEB
```wget \
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
  2. kalau sudah siap, tapi ada error `"run docker with --privileged"` : 
    a. keluar lingkungan docker
    b. commit pakai: `docker commit -m "pesan komit" -a "nama author" sha_container nama_image`
    c. jalankan kembali image tadi pakai `docker run -itp porthost:portdocker --privileged image:tag`
