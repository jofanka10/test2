## Soal_4

### A. Variabel Input
Dalam membaca input user, diperlukan sebuah inisialisasi variabel dengan format sebagai berikut.
```
$0 $1 $2 $3
```
dimana

1. ```$0``` Merupakan kode eksekusi program, seperti  ```./pokemon_analysis.sh```

2. ```$1``` Merupakan kode nama file, seperti ```pokemon_usage.csv```.
3. ```$2``` Merupakan kode untuk menjalankan sebuah fungsi, seperti ```--info```, ```--help``` dan alin sebagainya.
4. ```$3``` Merupakan kode untuk menjalankan fungsi tambahan, sebagai contoh pada fungsi ```--grep``` diperlukan fungsi tambahan, contohnya ```usage```

sehingga contoh penggunaan command sebagai berikut.
```
./pokemon_analysis.sh pokemon_usage.csv --grep usage
```


### B. Fungsi ```--help``` atau ```-h```
Fungsi ini untuk mencetak sebuah tampilan yang memudahkan user untuk mengetahui bagaimana cara menggunakan file tersebut.


### C. Fungsi ```--info```
Fungsi ```--info``` berfungsi untuk memberikan informasi berupa pokemon dengan usage dan RawUsage tertinggi.
```
  awk -F "," '
  BEGIN {usage="no_usage"; usage_num=0; rawusage="now_rawusage"; rawusage_num=0} 
  # Untuk sorting usage
  {if ($2 >= usage_num && NR>=2) usage_num = $2; usage=$1} 
  
  # Untuk sorting RawUsage
  {if ($3 >= rawusage_num && NR>=2) rawusage_num = $3; rawusage = $1} 
  END { print " User dengan usage paling tinggi = ", usage, "dengan", usage_num,"\n", "User dengan RawUsage tertinggi =", rawusage, "dengan", rawusage_num}' $file
```
dimana
1. ```BEGIN{...}``` untuk setting variabel awal, yaitu pokemon dan usage/rawUsage.
2. Fungsi dibawah ini untuk mencari data maksimum.
   ```
   {if ($2 >= usage_num && NR>=2) usage_num = $2; usage=$1}
   ```
   Dalam fungsi tersebut, jika jumlah bilangan pada kolom 2 ```$2``` melebihi atau sama dengan ```usage_num```, dan baris yang dituju lebih dari 2 (```NR>=2```) maka data tersebut menjadi data maksimum, begitu pula dengan rawUsage.

3. Untuk fungsi awk ```END{...}``` digunakan untuk mencetak output hasil akumulasi.


### D. Fungsi ```--sort```
Fungsi ini digunakan untuk menyortir data berdasarkan kolom tertentu.
```
function show_sort()
{
  head -n 1 "$file"
  case $column in
    ("pokemon") tail -n +2 "$file" | sort -t, -k1,1 -nr ;;
    ("usage") tail -n +2 "$file" | sort -t, -k2,2 -nr ;;
    ("rawusage") tail -n +2 "$file" | sort -t, -k3,3 -nr ;;
    ("type1") tail -n +2 "$file" | sort -t, -k4,4 -nr ;;
    ("type2") tail -n +2 "$file" | sort -t, -k5,5 -nr ;;
    ("hp") tail -n +2 "$file" | sort -t, -k6,6 -nr ;;
    ("atk") tail -n +2 "$file" | sort -t, -k7,7 -nr ;;
    ("def") tail -n +2 "$file" | sort -t, -k8,8 -nr ;;
    ("spatk") tail -n +2 "$file" | sort -t, -k9,9 -nr ;;
    ("spdef") tail -n +2 "$file" | sort -t, -k10,10 -nr ;;
    ("speed") tail -n +2 "$file" | sort -t, -k11,11 -nr ;;
    (*) echo "Input $column tidak ditemukan."
      echo "Input yang tersedia: pokemon, usage, rawusage, type1, type2, hp, atk, def, spatk, spdef, speed." ;;
  esac
}
```
1. Untuk fungsi ```head -n 1 "$file"``` digunakan untuk mencetak header dari sebuah data.
   
2. Untuk kode ini
   ```
   case $column in
    ("pokemon") tail -n +2 "$file" | sort -t, -k1,1 -nr ;;
    ..
    (*) echo "Input $column tidak ditemukan."
      echo "Input yang tersedia: pokemon, usage, rawusage, type1, type2, hp, atk, def, spatk, spdef, speed." ;;
   esac
   ```
  dimana:
  
  A. ```case $column in``` untuk mengambil data user berupa column
  
  B. ```("pokemon")``` merupakan penyortiran data berdasarkan nama.
  
  C. ```tail -n +2 "$file"``` merupakan pencetakan data dari ```$file```, dengan +2 agar data header tidak ikut dicetak. 
  
  D. ```| sort -t, -k1,1 -nr``` merupakan penyortiran dengan: ```-t,``` jeda menggunakan tanda koma, ```-k1,1``` merupakan penyortiran berdasarkan kolom pertama, dan ```-nr``` merupakan penyortiran data dilakukan secara descemnding.


### E. Fungsi ```--grep```
Untuk fungsi berikut
```
function search_pokemon()
{
  file=$1
  name=$2
  grep -i "^$name," $file
}

```
dimana:
1. ```file=$1``` merupakan inisialisasi file csv.
  
2. ```name=$2``` merupakan inisialisasi nama pokemon yang akan dicari.
   
3. ```grep -i "^$name," $file``` merupakan pencarian denan ```-i``` sebagai case-insensitive (tidak membedakan huruf besar/kecil).

### F. Fungsi ```-f``` dan ```--filter```
```
function type_filter()
{
  file=$1
  search=$2
  # untuk case insensitive
  awk -F, -v search="$search" 'tolower($4) == tolower(search) || tolower($5) == tolower(search)' $file | sort -k1
  
  # untuk case sensitive
  # awk -F, -v search="$search" '$4 == search || $5 == search' $file
}
 ```
1. ```file=$1``` merupakan inisialiasi file csv.
2. ```search=$2``` merupakan inisialisasi pencarian yang diinginkan.
3. ```awk -F,``` merupakan fungsi awk dengan koma sebagai pemisah.
4. ``` -v search="$search"``` merupakan fungsi agar mengambil sebuah variabel dari shell dan dimasukkan ke dalam fungsi awk.
5. ```'tolower($4) == tolower(search) || tolower($5) == tolower(search)'``` merupakan fungsi jika type yang dicari ada pada kolom 4 atau 5 secara case-insensitive.
6. ```sort -k1``` merupakan penyortiran berdasarkan nama pokemon.


### G. Fungsi ```case $command in```
1. Fungsi ```"-h"|"--help"```
   ```
     "-h"|"--help")
    show_help
    ;;
   ```
   Dari salah satu fungsi merupakan jika input user ```-h``` atau ```--help``` maka akan menjalankan fungsi ```show_help```.

2. Fungsi ```--sort```
    ```
     "--sort")
    if [ -z "$column" ]; then 
      echo "Input yang anda masukkan salah" 
      exit 1
    fi
      show_sort
    ;;
   ```
   Untuk fungsi ```--sort``` pada ```if [ -z "$column" ];``` merupakan case dimana jika input ```$column``` tidak ada, maka muncul pesan ```"Input yang anda masukkan salah"```, jika tidak maka akan menjalankan fungsi ```show_sort```.

3. Fungsi ```--grep```
   ```
     "--grep")
    if [ $# -lt 3 ]; then 
      echo "Kata yang anda cari tidak dimasukkan. Coba lagi"
      exit 1
    fi
       search_pokemon $file $3
    ;
   ```
   pada fungsi ```--grep```, jika input kurang dari 3, misal ```./pokemon_analysis.sh pokemon_usage.csv --grep``` maka muncul pesan ```Kata yang anda cari tidak dimasukkan. Coba lagi```, jika tidak akan menjalankan fungsi ```search_pokemon``` dengan input ```$file``` sebagai ```$1``` dan ```$3``` sebagi ```$2```.

## soal_1
Pada soal ini, program diminta untuk mengubah file txt hexadecimal menjadi sebuah gambar.
### a. Hex to Byte Converter
Fungsi ini digunakan untuk mengubah string hexadecimal menjadi format biner (byte). Untuk kodenya seperti ini
```
unsigned char hex_byte_converter(const char *hex) {
    unsigned char byte;
    sscanf(hex, "%2hhx", &byte);
    return byte;
}
```
Dimana fungsi akan mengconvert dengan kode 
```
sscanf(hex, "%2hhx", &byte);
```
lalu mengembalikan nilai berupa kode biner.

### b. Fungsi convert to Image
Fungsi ini digunakan untuk mengubah text menjadi gambar. Untuk kodenya seperti ini.
```
void convert_to_image(const char *filename) {
    printf(">> Memproses file: %s\n", filename);

    FILE *input = fopen(filename, "r");
    if (!input) {
        perror("Gagal membuka file input");
        return;
    }

    fseek(input, 0, SEEK_END);
    long length = ftell(input);
    rewind(input);

    if (length <= 0) {
        printf("!! File kosong: %s\n", filename);
        fclose(input);
        return;
    }

    char *hex_data = malloc(length + 1);
    fread(hex_data, 1, length, input);
    hex_data[length] = '\0';
    fclose(input);

    // Hapus semua newline dan carriage return
    char *clean_hex = malloc(length + 1);
    int j = 0;
    for (int i = 0; i < length; i++) {
        if (hex_data[i] != '\n' && hex_data[i] != '\r') {
            clean_hex[j++] = hex_data[i];
        }
    }
    clean_hex[j] = '\0';
    free(hex_data);

    size_t hex_len = strlen(clean_hex);
    if (hex_len % 2 != 0) {
        printf("!! Hex length ganjil, tidak valid: %s\n", filename);
        free(clean_hex);
        return;
    }

    size_t bin_len = hex_len / 2;
    unsigned char *bin_data = malloc(bin_len);
    if (!bin_data) {
        free(clean_hex);
        return;
    }

    for (size_t i = 0; i < bin_len; i++) {
        bin_data[i] = hex_byte_converter(&clean_hex[i * 2]);
    }
    free(clean_hex);

    // Buat folder image di dalam source_dir
    char image_dir[PATH_MAX];
    snprintf(image_dir, sizeof(image_dir), "%s/image", source_dir);
    struct stat st = {0};
    if (stat(image_dir, &st) == -1) {
        if (mkdir(image_dir, 0755) == -1) {
            perror("Gagal membuat folder image");
            free(bin_data);
            return;
        }
    }

    // Ambil nama file tanpa path dan ekstensi
    const char *base_name = strrchr(filename, '/');
    base_name = base_name ? base_name + 1 : filename;

    char name_part[128];
    sscanf(base_name, "%[^.]", name_part);

    time_t t = time(NULL);
    struct tm tm = *localtime(&t);

    char output_filename[PATH_MAX];
    snprintf(output_filename, sizeof(output_filename),
        "%s/%s_image_%04d-%02d-%02d_%02d-%02d-%02d.png",
        image_dir, name_part,
        tm.tm_year + 1900, tm.tm_mon + 1, tm.tm_mday,
        tm.tm_hour, tm.tm_min, tm.tm_sec);

    FILE *output = fopen(output_filename, "wb");
    if (output) {
        fwrite(bin_data, 1, bin_len, output);
        fclose(output);

    char log_path[PATH_MAX];
    snprintf(log_path, sizeof(log_path), "%s/conversion.log", source_dir);
    FILE *log = fopen(log_path, "a");
        if (log) {
            fprintf(log, "[%04d-%02d-%02d][%02d:%02d:%02d]: Successfully converted hexadecimal text %s to %s.\n",
                tm.tm_year + 1900, tm.tm_mon + 1, tm.tm_mday,
                tm.tm_hour, tm.tm_min, tm.tm_sec,
                filename, output_filename);
            fclose(log);
        }

        printf("✓ Converted: %s -> %s\n", filename, output_filename);
    } else {
        printf("!! Gagal membuat file gambar: %s\n", output_filename);
    }

    free(bin_data);

}
```
Dimana untuk cara kerjanya sebagai berikut.
1. Fungsi akan mengambil input berupa `*char filename`.
2. Setelah itu, fungsi akan membuka filename terebut dalam mode read.
3. Jika file tidak ada, maka muncul pesan `Gagal membuka file input`.
4. Setelah itu, file menentukan ukuran (panjang) dari file yang sedang dibuka.
5. Jika panjangnya <= 0, makka program akan muncul pesan `!! File kosong: %s`.
6. Program memuat seluruh konten file heksadesimal ke dalam memori untuk pemrosesan lebih lanjut, dan kemudian menutup file tersebut.
7. Program membersihkan data heksadesimal yang telah dibaca dari file (`hex_data`) dengan menghapus semua karakter newline (`\n`) dan carriage return (`\r`).
8. Setelah itu, memori yang berisi hex_data dibersihkan untuk emnghindari terjadinya memory leak.
9. Program memvalidasi panjang string heksadesimal.
10. Jika panjangnya ganjil, menandakan format tidak valid (karena 2 karakter hex = 1 byte biner), sehingga program akan muncul pesan `!! Hex length ganjil, tidak valid: %s`.
11. Fungsi menghitung ukutan data biner yang akan dihasilkan (setengah dari panjang hex) dan mengalokasikan memori yang diperlukan untuk menyimpannya.
12. menggunakan for loop, fugnsi ini menjalankan fungsi lain, yaitu fungsi `hex_byte_converter` pada poin a.
13. Lalu, fugnsi membuat folder image di dalam source_dir.
14. Jika gagal, maka muncul pesan error berupa `Gagal membuat folder image`.
15. Setelah itu, fungsi mengekstrak nama dasar file dari sebuah path lengkap.
16. Jika nama dasarnya tidak null, maka pointer `base_name` akan digeser saru posisi kedepan, melewati karakter `/` terakhir.
17. Jika nama dasarnya bernilai `NULL`, maka file tersebut sudah merupakan file dasar.
18. Kode ini mengekstraksi bagian nama file dari variabel base_name (yang sudah berisi nama file tanpa path direktori) hingga karakter titik (.) pertama ditemukan. Hasilnya disimpan di name_part.
19. Selanjutnya, fungsi mendapatkan waktu dan tanggal sistem saat ini.
20. Kemudian, fungsi membuat nama file lengkap untuk gambar output. Nama ini akan menggabungkan path direktori gambar `image_dir`), `name_part`, dan timestamp (tahun, bulan, hari, jam, menit, detik) ke dalam format `[direktori_image]/[nama_file]_image_[YYYY-MM-DD_HH-MM-SS].png`.
21. Fungsi ini mencoba membuka file output dengan nama yang telah dibuat dalam mode tulis biner ("wb").
22. Jika file output berhasil dibuka:
    - Data biner yang sudah dikonversi (bin_data) ditulis ke file gambar.
    - File gambar kemudian ditutup.
    -= Kode mencoba membuka file log bernama conversion.log (di dalam source_dir) dalam mode append ("a").
    - Jika file log berhasil dibuka, sebuah entri log yang berisi timestamp dan detail konversi (file input dan file output) ditulis ke dalamnya, lalu file log ditutup.
23. Jika file output gagal dibuka, pesan kesalahan akan dicetak ke konsol.
24. Terakhir, memori yang dialokasikan untuk `bin_data` dibebaskan.
