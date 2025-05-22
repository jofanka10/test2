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
24. Terakhir, memori yang dialokasikan untuk `bin_data` dibebaskan untuk menghindari terjadinya memory leak.

### c. Fungsi `is_converted`
Fungsi ini berfungsi untuk memeriksa apakah file telah dikonversi sebelumnya berdasarkan pathnya. Untuk kodenya seperti ini.
```
static bool is_converted(const char *filepath) {
    for (int i = 0; i < converted_count; i++) {
        if (strcmp(converted_files[i], filepath) == 0)
            return true;
    }
    return false;
}
```
Untuk cara kerjanya yaitu mulanya fungsi mengiterasi daftar konversi, lalu membndingkan path dan pengembalian true jika ditemukan, false jika ditemukan.

### d. Fungsi `mark_converted`
Fungsi mark_converted berfungsi untuk menambahkan path sebuah file ke dalam daftar (cache) file yang telah berhasil dikonversi. Untuk kodenya seperti ini.
```
static void mark_converted(const char *filepath) {
    if (converted_count < MAX_CONVERTED_FILES) {
        strncpy(converted_files[converted_count], filepath, PATH_MAX);
        converted_files[converted_count][PATH_MAX - 1] = '\0'; // safety null terminator
        converted_count++;
    }
}
```
Untuk cara kerjanya sebagai berikut.
1. Fungsi memeriksa apakah jumlah file yang sudah tercatat (`converted_count`) belum mencapai batas maksimum yang diizinkan (`MAX_CONVERTED_FILES`).
2. Jika kapasitas belum penuh, path file yang diberikan (`filepath`) akan disalin ke lokasi kosong berikutnya dalam array `converted_files`.
3. Setelah berhasil disalin, penghitung `converted_count` akan ditingkatkan satu untuk mencatat bahwa ada satu file lagi yang sudah ditandai.
4. Jika kapasitas `MAX_CONVERTED_FILES` sudah tercapai, fungsi ini tidak melakukan apa pun dan tidak menambahkan path file baru.

### e. `fs_getattr()`
Fungsi ini digunakan untuk mendapatkan atribut dari sebuah file atau direktori, seperti tipe (file atau direktori), izin akses, ukuran, dan jumlah hard link. Untuk kodenya seperti ini
```
static int fs_getattr(const char *path, struct stat *st, struct fuse_file_info *fi) {
    (void) fi;
    char realpath[PATH_MAX];
    snprintf(realpath, sizeof(realpath), "%s%s", source_dir, path);

    printf("[getattr] path=%s, realpath=%s\n", path, realpath);

    if (strcmp(path, "/") == 0) {
        st->st_mode = S_IFDIR | 0755;
        st->st_nlink = 2;
        return 0;
    }

    struct stat temp_stat;
    if (stat(realpath, &temp_stat) == -1)
        return -ENOENT;

    if (S_ISREG(temp_stat.st_mode)) {
        st->st_mode = S_IFREG | 0444;
        st->st_nlink = 1;
        st->st_size = temp_stat.st_size;
        return 0;
    }

    if (S_ISDIR(temp_stat.st_mode)) {
        st->st_mode = S_IFDIR | 0755;
        st->st_nlink = 2;
        return 0;
    }

    return -ENOENT;
}
```
dimana untuk cara kerjanya sebagai berikut.
1. Fungsi ini menggabungkan source_dir (direktori sumber asli) dengan path (jalur yang diminta oleh sistem FUSE) untuk membentuk jalur file yang sebenarnya di sistem operasi.
2. Jika path yang diminta adalah / (direktori root FUSE), fungsi ini secara langsung mengatur atributnya sebagai direktori dengan izin `0755`.
3. Memeriksa Keberadaan File: Untuk path lainnya, fungsi ini mencoba mendapatkan atribut dari file atau direktori nyata menggunakan `stat()`. Jika tidak ditemukan, fungsi mengembalikan `-ENOENT`.
4. Jika `stat()` berhasil, fungsi memeriksa apakah itu file biasa. Jika ya, atributnya diatur sebagai file dengan izin hanya baca (`0444`) dan ukurannya disalin.
5. Jika itu direktori, atributnya diatur sebagai direktori dengan izin `0755`.
6. Jika atribut berhasil ditentukan, fungsi mengembalikan 0 (berhasil). Jika tipe entri tidak didukung atau ada masalah lain, fungsi mengembalikan `-ENOENT`.

### f. `x_readdir()`
Fungsi ini digunakan untuk membaca sebuah direktori dalam FUSE. Untuk kodenya seperti ini
```
static int x_readdir(const char *path, void *buf, fuse_fill_dir_t filler,
                     off_t offset, struct fuse_file_info *fi,
                     enum fuse_readdir_flags flags)
{
    (void) offset;
    (void) fi;
    (void) flags;

    if (strcmp(path, "/") != 0)
        return -ENOENT;

    filler(buf, ".", NULL, 0, 0);
    filler(buf, "..", NULL, 0, 0);

    DIR *dp = opendir(source_dir);
    if (!dp)
        return -errno;

    struct dirent *de;
    while ((de = readdir(dp)) != NULL) {
        struct stat st;
        memset(&st, 0, sizeof(st));

        if (de->d_type == DT_DIR)
            st.st_mode = S_IFDIR | 0755;
        else if (de->d_type == DT_REG)
            st.st_mode = S_IFREG | 0444;
        else
            st.st_mode = S_IFREG | 0444;

        filler(buf, de->d_name, &st, 0, 0);
    }

    closedir(dp);
    return 0;
}
```
dimana untuk cara kerjanya sebagai berikut.
1. Fungsi memastikan path yang diminta adalah direktori root (`/`). Jika bukan, fungsi akan mengembalikan `-ENOENT`.
2. Fungsi menambahkan entri `.` (direktori saat ini) dan `..` (direktori induk) ke buffer daftar direktori.
3. Fungsi membuka direktori source_dir (direktori fisik yang dipetakan oleh FUSE) untuk membaca isinya. Jika gagal, fungsi mengembalikan error.
4. Fungsi mengulang setiap entri di `source_dir`. Untuk setiap entri, fungsi menentukan apakah itu direktori atau file biasa, menetapkan izin yang sesuai (`0755` untuk direktori, `0444` untuk file), dan menambahkannya ke buffer daftar direktori yang akan ditampilkan ke pengguna.
5. Setelah semua entri dibaca, fungsi menutup direktori sumber.
6. Fungsi mengembalikan `0` jika operasi berhasil.

### g. `fs_open()`
Fungsi ini berfungsi tidak hanya "membuka" file dalam arti tradisional, tetapi juga mengintegrasikan logika konversi heksadesimal ke gambar. Untuk kodenya seperti ini
```
static int fs_open(const char *path, struct fuse_file_info *fi) {
    char fullpath[PATH_MAX];
    snprintf(fullpath, sizeof(fullpath), "%s%s", source_dir, path);

    printf("[open] path=%s, realpath=%s\n", path, fullpath);

    int fd = open(fullpath, O_RDONLY);
    if (fd == -1) return -ENOENT;
    close(fd);

    if (!is_converted(fullpath)) {
        convert_to_image(fullpath);
        mark_converted(fullpath);
    }

    return 0;
}
```
dimana untuk cara kerjanya seperti ini.
1. Fungsi menggabungkan source_dir dan path FUSE untuk mendapatkan jalur file yang sebenarnya di sistem file fisik.
2. Fungsi mencoba membuka file fisik dalam mode hanya-baca.
3. Jika file tidak ada atau gagal dibuka, fungsi mengembalikan error berupa `-ENOENT`.
4. File segera ditutup kembali karena tujuan utama open FUSE ini bukan untuk menyimpan file descriptor.
5. Fungsi memeriksa apakah file sudah dikonversi menggunakan fungsi `is_converted()`.
6. ika belum dikonversi, fungsi memanggil fungsi `convert_to_image()` untuk mengubah konten heksadesimal file menjadi gambar. Setelah konversi, file tersebut ditandai sebagai sudah dikonversi menggunakan `mark_converted()`.
7. Fungsi mengembalikan `0` untuk menandakan bahwa operasi pembukaan file berhasil.

### `h. fs_read()`
Fungsi ini digunakan untuk membaca sebuah file dalam FUSE. Untuk kodenya seperti ini
```
static int fs_read(const char *path, char *buf, size_t size, off_t offset,
                   struct fuse_file_info *fi) {
    (void) fi;

    char fullpath[PATH_MAX];
    snprintf(fullpath, sizeof(fullpath), "%s%s", source_dir, path);

    printf("[read] path=%s, realpath=%s, size=%zu, offset=%ld\n", path, fullpath, size, offset);

    int fd = open(fullpath, O_RDONLY);
    if (fd == -1) return -errno;

    int res = pread(fd, buf, size, offset);
    if (res == -1) res = -errno;
    close(fd);
    return res;
}
```
dimana untuk cara kerjanya sebagai berikut.
1. Fungsi menggabungkan source_dir dan path FUSE untuk mendapatkan jalur file yang sebenarnya di sistem file fisik.
2. Fungsi membuka file fisik dalam mode hanya-baca. Jika gagal, fungsi mengembalikan error.
3. Fungsi membaca data dari file fisik dimulai dari offset tertentu dengan ukuran size yang diminta, lalu menyimpannya ke buffer yang disediakan (buf).
4. Setelah membaca, fungsi menutup file yang telah dibuka.
5. Fungsi mengembalikan jumlah byte yang berhasil dibaca, atau nilai error jika terjadi masalah selama pembacaan.

### `i. Fuse Operations`
Fungsi ini digunakan untuk menjalankan beberapa operasi yang diperlukan pada FUSE. Untuk kodenya seperti ini.
```
static struct fuse_operations fs_oper = {
    .getattr = fs_getattr,
    .readdir = x_readdir,
    .open = fs_open,
    .read = fs_read,
};
```

### `j. int main()`
Fungsi ini digunaka sebagai program utama. Untuk kodenya seperti ini.
```
int main(int argc, char *argv[]) {
    char cwd[PATH_MAX];
    if (getcwd(cwd, sizeof(cwd)) == NULL) {
        perror("getcwd() error");
        return 1;
    }
    snprintf(source_dir, sizeof(source_dir), "%s/anomali", cwd);
    printf("Source directory set to: %s\n", source_dir);

    return fuse_main(argc, argv, &fs_oper, NULL);
}
```
Dimana cara kerjanya sebagai berikut.
1. Program mencoba mendapatkan path direktori kerja saat ini (cwd) tempat program dijalankan.
2. Jika gagal, program akan mencetak kesalahan dan keluar.
3. Program membuat jalur absolut ke source_dir dengan menggabungkan direktori kerja saat ini dan sub-direktori "anomali".
4. Program memulai FUSE filesystem, menyerahkan kontrol ke library FUSE dengan argumen command-line yang diterima (argc, argv) dan struktur operasi (fs_oper) yang mendefinisikan perilaku filesystem.


## soal_2
Pada soal ini, program dapat menyatukan beberapa file yang terpecah menjadi 14 bagian dengan format .000, .001, sampai .013.

### `int_main()`
Untuk kodenya seperti ini
```
int main(int argc, char *argv[]) {
    char cwd[PATH_MAX];
    if (getcwd(cwd, sizeof(cwd)) == NULL) {
        perror("getcwd");
        return 1;
    }

    snprintf(relics_dir, sizeof(relics_dir), "%s/relics", cwd);
    snprintf(log_path, sizeof(log_path), "%s/activity.log", cwd);

    umask(0);
    return fuse_main(argc, argv, &baymax_oper, NULL);
}
```
Dimana untuk cara kerjanya sebagai berikut.
1. Fungsi ini mendapatkan direktori kerja saat ini untuk menentukan lokasi file-file penting.
2. Jalur ke direktori relics dan file activity.log diatur berdasarkan direktori kerja tersebut.
3. Izin default untuk file dan direktori baru disetel menggunakan umask(0).
4. Terakhir, fungsi ini memulai sistem file FUSE dan menyerahkan kontrol ke library FUSE.

### `write_log()`
Untuk write log kodenya seperti ini
```
void write_log(const char *format, ...) {
    FILE *logfile = fopen(log_path, "a");
    if (!logfile) return;

    time_t now = time(NULL);
    struct tm *tm_info = localtime(&now);

    char timebuf[64];
    strftime(timebuf, sizeof(timebuf), "[%Y-%m-%d %H:%M:%S]", tm_info);

    fprintf(logfile, "%s ", timebuf);

    va_list args;
    va_start(args, format);
    vfprintf(logfile, format, args);
    va_end(args);

    fprintf(logfile, "\n");
    fclose(logfile);
}
```
Dimana untuk cara kerjanya seperti ini
1. Fungsi ini membuka file log (activity.log) dalam mode append.
2. Waktu saat ini diambil dan diformat menjadi timestamp.
3. Timestamp tersebut ditulis ke file log, diikuti oleh pesan yang diformat.
4. Akhirnya, file log ditutup untuk memastikan data tersimpan.

### Fungsi `make_part_path()`
Untuk kodenya seperti ini.
```
static void make_part_path(char *buf, size_t size, const char *filename, int index) {
    snprintf(buf, size, "%s/%s.%03d", relics_dir, filename, index);
}
```
Dimana cara kerja dari kode ini adalah fungsi ini membangun jalur lengkap ke file fragmen fisik dengan menggabungkan direktori relics, nama file virtual, dan indeks fragmen.

### Fungsi `count_parts()`
Untuk kodenya seperti ini.
```
static int count_parts(const char *filename) {
    char part_path[PATH_MAX];
    int count = 0;
    while (count < MAX_PARTS) {
        make_part_path(part_path, sizeof(part_path), filename, count);
        if (access(part_path, F_OK) != 0) break;
        count++;
    }
    return count;
}
```
Dimana untuk cara kerjanya seperti ini.
1. Fungsi ini melakukan iterasi untuk memeriksa keberadaan setiap fragmen file (misalnya .000, .001, dst.).
2. Penghitungan fragmen berhenti saat file fragmen tidak ditemukan lagi.
3. Jumlah total fragmen yang ditemukan dikembalikan.

### fungsi `read_full_file()`
Untuk kodenya seperti ini.
```
static int read_full_file(const char *filename, char *buf, size_t size, off_t offset) {
    int part_count = count_parts(filename);
    if (part_count == 0) return -ENOENT;

    size_t total_size = part_count * MAX_PART_SIZE;
    if (offset >= total_size) return 0;

    size_t to_read = size;
    if (offset + to_read > total_size)
        to_read = total_size - offset;

    size_t read_bytes = 0;
    int part_index = offset / MAX_PART_SIZE;
    off_t part_offset = offset % MAX_PART_SIZE;

    while (to_read > 0 && part_index < part_count) {
        char part_path[PATH_MAX];
        make_part_path(part_path, sizeof(part_path), filename, part_index);

        FILE *f = fopen(part_path, "rb");
        if (!f) return -EIO;

        if (fseek(f, part_offset, SEEK_SET) != 0) {
            fclose(f);
            return -EIO;
        }

        size_t can_read = MAX_PART_SIZE - part_offset;
        if (can_read > to_read) can_read = to_read;

        size_t n = fread(buf + read_bytes, 1, can_read, f);
        fclose(f);

        if (n == 0) break;

        read_bytes += n;
        to_read -= n;
        part_index++;
        part_offset = 0;
    }
    return read_bytes;
}
```
Dimana untuk cara kerjanya seperti ini.
1. Fungsi ini menentukan jumlah fragmen dan ukuran total file virtual.
2. Jika offset berada di luar ukuran file, fungsi mengembalikan nol byte yang dibaca.
3. Fungsi ini menghitung fragmen awal dan offset di dalamnya dari mana harus mulai membaca.
4. Sebuah loop membuka setiap fragmen yang relevan, mencari posisi yang benar, dan membaca data ke dalam buffer yang disediakan.
5. Loop melanjutkan ke fragmen berikutnya sampai semua data yang diminta terbaca atau tidak ada lagi fragmen.
6. Jumlah total byte yang berhasil dibaca dikembalikan.

### Fungsi `baymax_getattr()`
Fungsi ini digunakan untuk mendapatkan atribut file atau direktori, seperti tipe, izin, dan ukuran, yang diminta oleh kernel. Untuk kodenya seperti ini
```
static int baymax_getattr(const char *path, struct stat *stbuf, struct fuse_file_info *fi) {
    (void) fi;
    memset(stbuf, 0, sizeof(struct stat));

    if (strcmp(path, "/") == 0) {
        stbuf->st_mode = S_IFDIR | 0755;
        stbuf->st_nlink = 2;
        return 0;
    }

    const char *filename = path + 1;
    int parts = count_parts(filename);
    if (parts == 0) return -ENOENT;

    stbuf->st_mode = S_IFREG | 0644;
    stbuf->st_nlink = 1;
    stbuf->st_size = parts * MAX_PART_SIZE;
    return 0;
}
```
Dimana untuk cara kerjanya seperti ini.
1. Fungsi ini menginisialisasi struktur atribut (stbuf) ke nol.
2. Jika path adalah direktori root (/), atribut diatur untuk direktori (0755).
3. Jika path adalah file, jumlah fragmen dihitung.
4. Jika file tidak memiliki fragmen, fungsi mengembalikan error ENOENT.
5. Atribut file (0644, ukuran total dari fragmen) diatur dan dikembalikan.

### `baymax_readdir()`
Fungsi ini digunakan untuk membaca isi direktori (dalam kasus ini, hanya direktori root) dan mengembalikan daftar file yang terlihat. Untuk kodenya seperti ini
```
static int baymax_readdir(const char *path, void *buf, fuse_fill_dir_t filler, off_t offset,
                          struct fuse_file_info *fi, enum fuse_readdir_flags flags) {
    (void) offset; (void) fi; (void) flags;

    if (strcmp(path, "/") != 0)
        return -ENOENT;

    filler(buf, ".", NULL, 0, 0);
    filler(buf, "..", NULL, 0, 0);

    DIR *d = opendir(relics_dir);
    if (!d) return 0;

    char lastname[512] = {0};
    struct dirent *entry;
    while ((entry = readdir(d)) != NULL) {
        if (entry->d_name[0] == '.') continue;

        int len = strlen(entry->d_name);
        if (len < 4) continue;
        if (entry->d_name[len - 4] != '.') continue;

        char base[512];
        strncpy(base, entry->d_name, len - 4);
        base[len - 4] = '\0';

        if (strcmp(base, lastname) != 0) {
            if (filler(buf, base, NULL, 0, 0) != 0)
                break;
            strcpy(lastname, base);
        }
    }

    closedir(d);
    return 0;
}
```
Dimana untuk cara kerjanya sebagai berikut.
1. Fungsi ini hanya menangani pembacaan untuk direktori root (/); selain itu, ia mengembalikan error.
2. Entri standar . dan .. ditambahkan ke buffer direktori.
3. Fungsi ini membuka direktori relics untuk membaca file fragmen fisik.
4. Sebuah loop membaca setiap entri di direktori relics.
5. File-file yang bukan fragmen atau yang tersembunyi dilewati.
6. Nama dasar file (tanpa ekstensi fragmen) diekstraksi dari nama fragmen.
7. Hanya nama dasar file yang unik ditambahkan ke buffer yang akan dilihat oleh pengguna.
8. Setelah selesai, direktori relics ditutup.

### `baymax_open()`
Fungsi ini digunakan ketika aplikasi pengguna mencoba membuka sebuah file. Untuk kodenya seperti ini.
```
static int baymax_open(const char *path, struct fuse_file_info *fi) {
    const char *filename = path + 1;
    int parts = count_parts(filename);
    if (parts == 0) return -ENOENT;
    return 0;
}
```
Dimana untuk cara kerjanya seperti ini.
1. Fungsi ini mengekstrak nama file virtual dari path.
2. Jumlah fragmen file dihitung untuk memeriksa keberadaan file.
3. Jika file tidak ditemukan (0 fragmen), fungsi mengembalikan error ENOENT.
4. Jika file ditemukan, fungsi mengembalikan 0 untuk menandakan sukses.

### `baymax_read()`
Fungsi ini digunakan ketika aplikasi pengguna mencoba membaca data dari sebuah file. Untuk kodenya seperti ini.
```
static int baymax_read(const char *path, char *buf, size_t size, off_t offset, struct fuse_file_info *fi) {
    (void) fi;
    const char *filename = path + 1;
    int res = read_full_file(filename, buf, size, offset);
    if (res >= 0) {
        write_log("READ: %s", filename);
    }
    return res;
}
```
Dimana untuk cara kerjanya seperti ini.
1. Fungsi ini mengekstrak nama file virtual dari path.
2. Fungsi helper read_full_file dipanggil untuk membaca data dari fragmen ke buffer yang disediakan.
3. Jika pembacaan berhasil, aktivitas "READ" dicatat ke log.
4. Hasil pembacaan (jumlah byte atau kode error) dikembalikan.

### `baymax_create()`
Fungsi ini digunakan ketika aplikasi pengguna mencoba membuat file baru. Untuk kodenya seperti ini.
```
static int baymax_create(const char *path, mode_t mode, struct fuse_file_info *fi) {
    (void) mode; (void) fi;
    return 0;
}
```
Dimana untuk cara kerjanya yaitu: Fungsi ini langsung mengembalikan 0 (sukses), lalu pembuatan fragmen file fisik ditunda hingga operasi penulisan terjadi.

### `file_write_buffer()`
Struktur dan fungsi-fungsi helper ini digunakan untuk mengelola buffer data di memori sementara file sedang ditulis, sebelum disimpan ke fragmen fisik. Untuk kodenya seperti ini.
```
struct file_write_buffer {
    char *data;
    size_t size;
    size_t capacity;
};

static struct file_write_buffer *get_write_buffer(struct fuse_file_info *fi) {
    return (struct file_write_buffer *)(uintptr_t)fi->fh;
}

static void set_write_buffer(struct fuse_file_info *fi, struct file_write_buffer *buf) {
    fi->fh = (uint64_t)(uintptr_t)buf;
}
```
Dimana untuk cara kerjanya seperti ini.
1. Struktur file_write_buffer mendefinisikan buffer memori untuk menyimpan data file yang sedang ditulis.
2. Fungsi get_write_buffer mengambil buffer tulis yang terkait dengan file handle FUSE.
3. Fungsi set_write_buffer menyimpan buffer tulis ke dalam file handle FUSE.

### `baymax_write()`
Fungsi ini digunakan ketika aplikasi pengguna mencoba menulis data ke sebuah file. Untuk kodenya seperti ini.
```
static int baymax_write(const char *path, const char *buf, size_t size, off_t offset, struct fuse_file_info *fi) {
    struct file_write_buffer *wb = get_write_buffer(fi);
    if (!wb) {
        wb = malloc(sizeof(struct file_write_buffer));
        if (!wb) return -ENOMEM;
        wb->data = NULL;
        wb->size = 0;
        wb->capacity = 0;
        set_write_buffer(fi, wb);
    }

    size_t newsize = offset + size;
    if (newsize > wb->capacity) {
        size_t newcap = newsize + 1024;
        char *newdata = realloc(wb->data, newcap);
        if (!newdata) return -ENOMEM;
        wb->data = newdata;
        wb->capacity = newcap;
    }

    memcpy(wb->data + offset, buf, size);
    
    if (newsize > wb->size) wb->size = newsize;

    return size;
}
```
Dimana untuk cara kerjanya sebagai berikut.
1. Fungsi ini mendapatkan atau mengalokasikan buffer tulis untuk file yang sedang dibuka.
2. Jika buffer terlalu kecil, kapasitas buffer akan diperluas.
3. Data yang diterima disalin ke dalam buffer memori.
4. Ukuran data efektif di buffer diperbarui.
5. Jumlah byte yang berhasil "ditulis" ke buffer dikembalikan.

### `save_parts()`
Fungsi ini bertanggung jawab untuk mengambil data dari buffer memori dan menulisnya ke fragmen-fragmen fisik di direktori relics. Untuk kodenya seperti ini
```
static int save_parts(const char *filename, const char *buf, size_t size) {
    int parts = 0;
    size_t offset = 0;

    while (offset < size) {
        size_t chunk = MAX_PART_SIZE;
        if (size - offset < chunk) chunk = size - offset;

        char part_path[PATH_MAX];
        make_part_path(part_path, sizeof(part_path), filename, parts);

        FILE *f = fopen(part_path, "wb");
        if (!f) return -EIO;

        size_t written = fwrite(buf + offset, 1, chunk, f);
        fclose(f);
        if (written != chunk) return -EIO;

        parts++;
        offset += chunk;
    }

    char part_path[PATH_MAX];
    while (true) {
        make_part_path(part_path, sizeof(part_path), filename, parts);
        if (access(part_path, F_OK) != 0) break;
        remove(part_path);
        parts++;
    }

    return parts;
}
```
Dimana untuk cara kerjanya seperti ini.
1. Fungsi ini melakukan iterasi untuk menulis data ke fragmen-fragmen fisik.
2. Setiap fragmen dibuka atau dibuat, dan data ditulis ke dalamnya.
3. Setelah penulisan selesai, sebuah loop terpisah menghapus fragmen-fragmen lama yang mungkin melebihi ukuran file baru.
4. Jumlah fragmen yang ditulis dikembalikan.

### `baymax_release()`
Fungsi ini digunakan ketika aplikasi pengguna selesai menggunakan file dan menutupnya. Ini adalah momen krusial untuk menyimpan data yang di-buffer ke disk. Untuk kodenya seperti ini.
```
static int baymax_release(const char *path, struct fuse_file_info *fi) {
    struct file_write_buffer *wb = get_write_buffer(fi);
    if (wb) {
        const char *filename = path + 1;
        int parts = save_parts(filename, wb->data, wb->size);
        if (parts > 0) {
            char part_list[4096] = {0};
            for (int i = 0; i < parts; i++) {
                char partname[256];
                snprintf(partname, sizeof(partname), "%s.%03d", filename, i);
                strcat(part_list, partname);
                if (i != parts - 1) strcat(part_list, ", ");
            }
            write_log("WRITE: %s -> %s", filename, part_list);
        }

        free(wb->data);
        free(wb);
        set_write_buffer(fi, NULL);
    }
    return 0;
}
```
Dimana untuk cara kerjanya seperti ini.
1. Fungsi ini mendapatkan buffer tulis yang terkait dengan file.
2. Jika ada buffer tulis, data dari buffer disimpan ke fragmen-fragmen fisik.
3. Jika ada fragmen yang ditulis, aktivitas "WRITE" dicatat ke log.
4. Memori yang dialokasikan untuk buffer dan datanya dibebaskan.
5. Fungsi mengembalikan 0 untuk menandakan sukses.

### `baymax_unlink()`
Fungsi ini digunakan ketika aplikasi pengguna mencoba menghapus (menghubungkan) sebuah file dari sistem file. Untuk kodenya seperti ini.
```
static int baymax_unlink(const char *path) {
    const char *filename = path + 1;
    char part_path[PATH_MAX];
    char part_list[4096] = {0};
    int index = 0;

    while (true) {
        make_part_path(part_path, sizeof(part_path), filename, index);
        if (access(part_path, F_OK) != 0) break;

        char partname[64];
        snprintf(partname, sizeof(partname), "%s.%03d", filename, index);
        if (index > 0) strcat(part_list, ", ");
        strcat(part_list, partname);

        remove(part_path);
        index++;
    }

    if (index > 0) {
        write_log("DELETE: %s", part_list);
    }

    return 0;
}
```
Dimana untuk cara kerjanya sebagai berikut.
1. Fungsi ini mengambil nama file virtual yang akan dihapus.
2. Sebuah loop menghapus semua fragmen fisik yang terkait dengan file tersebut.
3. Nama-nama fragmen yang dihapus ditambahkan ke daftar untuk log.
4. Jika ada fragmen yang dihapus, aktivitas "DELETE" dicatat ke log.
5. Fungsi mengembalikan 0 untuk menandakan sukses.

### Fuse Operations
Struktur ini adalah inti dari FUSE, memetakan operasi sistem file standar ke fungsi-fungsi kustom yang diimplementasikan dalam Baymax. Untuk kodenya seperti ini.
```
static struct fuse_operations baymax_oper = {
    .getattr = baymax_getattr,  // Mendapatkan atribut file/direktori.
    .readdir = baymax_readdir,  // Membaca isi direktori.
    .open    = baymax_open,     // Membuka file.
    .read    = baymax_read,     // Membaca dari file.
    .write   = baymax_write,    // Menulis ke file.
    .create  = baymax_create,   // Membuat file baru.
    .release = baymax_release,  // Melepaskan (menutup) file.
    .unlink  = baymax_unlink,   // Menghapus file.
};
```
