

## No. 14
Pada soal ini, kita akan mencari berapa banyak paket dalam file `.pcapng`. Untuk mencarinya kita hanya perlu melihat bagian bawah dari Wireshark. Untuk tampilannya seperti ini.

<img width="142" height="50" alt="Screenshot 2025-10-04 at 10 24 44" src="https://github.com/user-attachments/assets/a91ec772-41c8-4992-98ee-d8ac09b6f015" />

Selanjutnya, kita akan 

## No. 16
Pada soal ini, kita akan mencari username dan password yang ada pada file pcap.
<img width="1470" height="859" alt="Screenshot 2025-10-04 at 07 44 18" src="https://github.com/user-attachments/assets/1e496a5c-8491-4219-8631-9b3b699b626a" />

Lalu, kita akan mencari file-file yang ada di dalamnya. Jika dilihat dari hasil follow TCPnya, kita dapat menemukan lima file.

![Desain tanpa judul_2](https://github.com/user-attachments/assets/02b5bfa4-8bb5-4c7c-8ac6-aa9bee7586a0)

Setelah itu, kita akan mencari sha256 dari 5 file. Kita menggunakan q.exe di sini menggunakan fitur find di Wireshark.

<img width="1419" height="302" alt="Screenshot 2025-10-04 at 07 54 57" src="https://github.com/user-attachments/assets/0feec6b3-0c6e-4fb8-9124-c7fea509eba4" />

Setelah itu, kita follow TCP stream, lalu ubah tampilan datanya menjadi `RAW`. Setelah itu kita save as `.exe`.

<img width="1409" height="529" alt="Screenshot 2025-10-04 at 07 58 08" src="https://github.com/user-attachments/assets/cb6bf51d-8a99-4dd0-a089-ed4fa6137c6e" />

Kita buka terminal, lalu kita command `sha256 <namafile>`. Untuk hasilnya seperti ini.

<img width="1397" height="388" alt="Screenshot 2025-10-04 at 08 00 02" src="https://github.com/user-attachments/assets/6eddc8e8-c284-4412-9707-1a1a499321b0" />

Setelah semua datanya kita dapatkan, kita masukkan ke `nc 10.15.43.32 3403`. Untuk hasilnya seperti ini.

<img width="969" height="713" alt="Screenshot 2025-10-04 at 08 00 57" src="https://github.com/user-attachments/assets/67c368d9-3049-4ac5-b12e-7eead395486d" />

## No. 18
Pada soal ini, kita akan mencari berapa file yang mengandung malware, dan ditemukan ada dua file.

![Wireshark19_1](https://github.com/user-attachments/assets/795794bd-0d57-4abe-9356-d091ecf5b478)

Setelah itu, kita follow TCPnya, lalu kita find dengan kata kunci `.exe`.

<img width="714" height="294" alt="Screenshot 2025-10-03 at 12 50 48" src="https://github.com/user-attachments/assets/848d1ac3-8ac8-440e-99fb-58452c7ffacc" />

<img width="713" height="293" alt="Screenshot 2025-10-03 at 12 51 30" src="https://github.com/user-attachments/assets/4b2445bb-e402-44fa-a783-e77c04cde480" />

Lalu, kita akan mencari sha256 dari masing-masing file. Caranya yaitu dengna melakukan `File -> Export Objects -> SMB`, lalu muncul tampilan seperti di bawah, lalu save sebagai file exe.

<img width="838" height="554" alt="Screenshot 2025-10-03 at 13 31 43" src="https://github.com/user-attachments/assets/0337f759-f952-4d21-a20c-25d9278b79e4" />

Setelah itu, kita coba cari code sha256 melalui terminal. Untuk command dan hasilnya seperti ini.
<img width="1435" height="185" alt="Screenshot 2025-10-03 at 13 33 38" src="https://github.com/user-attachments/assets/2b993954-aede-436c-944b-cbfec2fa2750" />

Lalu kita masukkan ke `nc 10.15.43.32 3405` sehingga mendapatkan flag. Untuk hasilnya seperti ini.

<img width="1251" height="718" alt="Screenshot 2025-10-03 at 13 35 28" src="https://github.com/user-attachments/assets/0d2c405d-d05d-4be7-a384-c44e01761bea" />


## No. 19 

pada soal ini, kita akan mencari user yang menyerang. 

Caranya dengan menggunakan fitur search di Wiresharl (shortcut: `cmd + f` atau `ctrl + f`) lalu ketik "user". Setelah itu kita melakukan follow TCP untuk emngetahui detailnya. Untuk hasilnya seperti pada gambar di bawah.
<img width="1089" height="157" alt="Image" src="https://github.com/user-attachments/assets/8d895124-f711-4009-a1b4-076e0cf7c45e" />

<img width="714" height="491" alt="Screenshot 2025-10-03 at 12 34 05" src="https://github.com/user-attachments/assets/8393fa1e-c2ee-4e48-aba8-901eeb78689a" />
<img width="716" height="490" alt="Screenshot 2025-10-03 at 12 39 17" src="https://github.com/user-attachments/assets/12f8da6c-0b41-4172-a555-91670038f7f5" />


Dari sini kita dapat usernya, yaitu `Your Life`. Setelah datanya kita dapatkan kita masukkan ke `nc 10.15.43.32 3406`. Selanjutnya, kita input data yang ada di screenshot tersebut hingga kita dapat menemukan sebuah flag. Untuk hasilnya seperti ini.
<img width="1437" height="762" alt="Screenshot 2025-10-03 at 12 37 31" src="https://github.com/user-attachments/assets/5cebb95d-a75a-45b7-b123-8f62e9d06bb7" />
