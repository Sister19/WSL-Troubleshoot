# Troubleshooting Windows subsystem for linux untuk pengerjaan OS
## Instalasi WSL
![HelloWorld WSL](img/wsl-hello.jpg)
[Windows subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/about) atau disingkat sebagai WSL secara singkat
memperbolehkan pengguna Windows untuk menjalankan alat-alat sering digunakan pada distribusi Linux seperti `grep` dan
pipe redirection yang lebih powerful dibandingkan redirection milik `cmd` Windows.

Direkomendasikan untuk memasang WSL 2 dibandingkan WSL 1. WSL 2 dapat menjalankan 32-bit `ELF` executable dan sebagian besar
utility berfungsi secara normal pada WSL 2.

Instalasi WSL 2 dapat dicek pada link
[https://docs.microsoft.com/en-us/windows/wsl/install-win10](https://docs.microsoft.com/en-us/windows/wsl/install-win10).
Pastikan Windows 10 telah terupdate sebelum menjalankan instruksi pada link diatas. Untuk distribusi linux, direkomendasikan
memasang distribution **Ubuntu 20.04**.



## Instalasi X server
1. Pasang [VcXsrv](https://sourceforge.net/projects/vcxsrv/) atau X server lainnya.
2. Carilah folder shortcut VcXsrv yang berada pada Start Menu seperti `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\VcXsrv`.
3. Copy shortcut `XLaunch` dan berikan nama seperti `WSL-VcXsrv` atau yang lain.
4. Klik kanan dan pilih properties pada `WSL-VcXsrv`.

![Property image](img/wsl-property.jpg)

5. Masukkan parameter berikut pada kolom target, `"C:\Program Files\VcXsrv\vcxsrv.exe" :0 -ac -terminate -lesspointer -multiwindow -clipboard -wgl -dpi auto`. Pastikan program path sama dengan lokasi executable VcXsrv.

![Target image](img/wsl-target.jpg)

6. Jalankan WSL dan ketik kode berikut pada terminal `echo "export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0" >> ~/.bashrc`

![WSL DISPLAY](img/wsl-display.jpg)

7. Jalankan shortcut yang dibuat pada step 4 setiap kali ingin menjalankan `bochs`.
Jika ingin mengetes apakah instalasi berhasil pasanglah `xeyes` dengan package manager yang terdapat pada distribusi
(contoh pada ubuntu `sudo apt-get install xeyes`) dan jalankan command `xeyes` pada WSL.

![WSL xeyes](img/wsl-xeyes.jpg)



## Bochs berhenti dan tidak terlihat
Jika `bochs` masih belum terlihat tetapi tidak mengalami error seperti berikut

![WSL hidden](img/wsl-invisible.jpg)

Buka 2 terminal WSL, jalankan `sudo vim ~/.bashrc` pada terminal pertama dan `sudo vim /etc/resolv.conf`.
Pastikan konfigurasi antara `nameserver` dan `export DISPLAY` sama seperti berikut

![WSL vim bashrc](img/wsl-bashrc.jpg)

Setelah melakukan pengeditan, lakukan save dan exit pada editor `vim` (tekan esc, ketik ":wq").
Tutup semua terminal WSL dan jalankan ulang untuk reload konfigurasi bash.


## Bochs mengalami buffer overflow
Jika `bochs` mengalami buffer overflow seperti berikut

![WSL bof](img/wsl-bof.jpg)

Salah satu *workaround* yang dapat digunakan adalah menambahkan privilege `sudo` pada `bochs`.
