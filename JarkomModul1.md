[01/10/25, 22.00.02] Jofanka Al Kautsar P A: ‎Read more
[01/10/25, 23.33.40] Jofanka Al Kautsar P A: ## No. 14
Pada soal ini, praktikan diminta untuk mencari jumlah packet. Jumlah packetnya adalah 
## No. 18
Pada soal ini, kita akan mencari berapa file yang mengandung malware, dan ditemukan ada dua file.

![Wireshark19_1](https://github.com/user-attachments/assets/795794bd-0d57-4abe-9356-d091ecf5b478)

Setelah itu, kita follow TCPnya, lalu kita find dengan kata kunci `.exe`.

<img width="714" height="294" alt="Screenshot 2025-10-03 at 12 50 48" src="https://github.com/user-attachments/assets/848d1ac3-8ac8-440e-99fb-58452c7ffacc" />

<img width="713" height="293" alt="Screenshot 2025-10-03 at 12 51 30" src="https://github.com/user-attachments/assets/4b2445bb-e402-44fa-a783-e77c04cde480" />

Lalu, kita akan mencari sha256 dari masing-masing file. Caranya yaitu dengna melakukan `File -> Export Objects -> SMB`, lalu muncul tampilan seperti di bawah, lalu save sebagai file exe.

<img width="842" height="554" alt="Screenshot 2025-10-03 at 13 26 42" src="https://github.com/user-attachments/assets/951e7c58-8743-4ad5-b7da-3947c982d656" />



## No. 19 

pada soal ini, kita akan mencari user yang menyerang. 

Caranya dengan menggunakan fitur search di Wiresharl (shortcut: `cmd + f` atau `ctrl + f`) lalu ketik "user". Setelah itu kita melakukan follow TCP untuk emngetahui detailnya. Untuk hasilnya seperti pada gambar di bawah.
<img width="1089" height="157" alt="Image" src="https://github.com/user-attachments/assets/8d895124-f711-4009-a1b4-076e0cf7c45e" />

<img width="714" height="491" alt="Screenshot 2025-10-03 at 12 34 05" src="https://github.com/user-attachments/assets/8393fa1e-c2ee-4e48-aba8-901eeb78689a" />
<img width="716" height="490" alt="Screenshot 2025-10-03 at 12 39 17" src="https://github.com/user-attachments/assets/12f8da6c-0b41-4172-a555-91670038f7f5" />


Dari sini kita dapat usernya, yaitu `Your Life`. Setelah datanya kita dapatkan kita masukkan ke `nc 10.15.43.32 3406`. Selanjutnya, kita input data yang ada di screenshot tersebut hingga kita dapat menemukan sebuah flag. Untuk hasilnya seperti ini.
<img width="1437" height="762" alt="Screenshot 2025-10-03 at 12 37 31" src="https://github.com/user-attachments/assets/5cebb95d-a75a-45b7-b123-8f62e9d06bb7" />
