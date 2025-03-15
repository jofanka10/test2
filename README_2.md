## soal_3

### A. Fungsi ```Speak to Me```

Untuk menjalankan fungsi tersebut, pengguna diminta untuk memasukkan berapa lama fungsi ini berjalan (dalam detik).
```
read -p "Masukkan durasi script berjalan (dalam detik): " duration
```
Setelah itu, fungsi akan menjalankan afirmasi. Tetapi sebelum itu diperlukan best case dengan kode sebagai berikut.
```
    if ! [[ "$duration" =~ ^[0-9]+$ ]]; then
        echo -e "${RED}Masukkan angka yang valid.${NC}"
        exit 1
    fi
```
Dimana kode ini mengecek apakah input durasi berupa angka. Jika input bukan merupakan angka, maka akan muncul pesan berwarna merah yaitu ```Masukkan angka yang valid.```. Untuk fungsi ```{NC}``` sendiri merupakan fungsi untuk mereset warna agar warna teks dapat digunakan kembali.
Contoh output-nya sebagai berikut.

![Image](https://github.com/user-attachments/assets/ca9b44f2-2a67-4c53-9175-c9ff67e201cf)

Jika inputnya berupa angka (dalam detik) maka fungsi afirmasi akan berjalan sebagai berikut.
```
    start_time=$(date +%s)
    while [ $(($(date +%s) - start_time)) -lt $duration ]; do
        # Mengambil afirmasi dari API
        affirmation=$(curl -s -H "Accept: application/json" "https://www.affirmations.dev" | jq -r '.affirmation')
        
        # Memilih warna acak
        colors=("$RED" "$GREEN" "$YELLOW" "$BLUE" "$CYAN")
        color=${colors[$RANDOM % ${#colors[@]}]}
        
        # Menampilkan afirmasi dengan warna
        echo -e "${color}$affirmation${NC}"
        
        # Tunggu 1 detik
        sleep 1
    done

    echo -e "${GREEN}Selesai! Terima kasih telah menggunakan script ini.${NC}"
```
Jika fungsi ini dijalankan, maka outputnya sebagai berikut.

![Image](https://github.com/user-attachments/assets/f2525c4b-0972-4378-ac3a-3ea4d126f05a)


### B. Fungsi ```On the Run```

Untuk menjalankan fungsi tersebut, pada awalnya diperlukan sebuah kode yang bernama ```clear``` untuk menghapus isi terminal tersebut. Setelah itu, fungsi akan menginisialisasi warna teks (dalam fungsi tersebut menggunakan warna ASCII hijau ```\033[0;32m``` dan reset color ```\033[0m```. Lalu fungsi akan menginialisasi variabel awal berupa ```BAR_LENGTH=30``` dan ```PROGRESS=0```.
Setelah itu, fungsi ```draw_progress_bar()``` dijalankan untuk mencetak setiap proses per seratus persen.
```
    draw_progress_bar() {
        local filled=$((PROGRESS * BAR_LENGTH / 100))
        local empty=$((BAR_LENGTH - filled))

        # Buat progress bar
        BAR="["
        for ((i = 0; i < filled; i++)); do
            BAR+="${GREEN}#${NC}"
        done
        for ((i = 0; i < empty; i++)); do
            BAR+=" "
        done
        BAR+="]"

        # Tampilkan progress bar dan persentase
        echo -ne "\r${BAR} ${PROGRESS}%"
    }
```

Fungsi ```draw_progress_bar``` digunakan untuk dipanggil oleh fungsi while di bawah.
```
    while [ $PROGRESS -lt 100 ]; do
        # Tampilkan progress bar
        draw_progress_bar

        # Tentukan pesan status
        if [ $PROGRESS -lt 50 ]; then
            MESSAGE="sabar wir"
        elif [ $PROGRESS -lt 100 ]; then
            MESSAGE="dikit lagi bre"
        else
            MESSAGE="done yagesya"
        fi

        # Tampilkan pesan status di bawah progress bar
        tput el  # Hapus baris sebelumnya
        echo -e "\n${MESSAGE}"

        # Random delay (0.1 - 1 detik)
        sleep 0.$((RANDOM % 10))

        # Increment progress (acak antara 1-10)
        PROGRESS=$((PROGRESS + RANDOM % 10))
        if [ $PROGRESS -gt 100 ]; then
            PROGRESS=100
        fi

        # Geser kursor ke atas untuk menimpa pesan status sebelumnya
        tput cuu 2
    done
```
Untuk dapat menampilkan keseluruhan proses, kode di bawah diperlukan sebagai berikut.
```
    draw_progress_bar
    tput el  # Hapus baris sebelumnya
    MESSAGE="done yagesya"  # Pastikan pesan status diperbarui
    echo -e "\n${GREEN}${MESSAGE}${NC}"
```
Contoh outputnya seperti ini

https://github.com/user-attachments/assets/196b1fd6-6af9-429d-a0fc-7da511ed1af3

### C. Fungsi ```Time and Date```
Ketika fungsi ini dijalankan, akan muncul sebuah tampilan kapan tanggak dan waktu untuk saat ini. Untuk kodenya seperti ini

```
    echo "Menjalankan Time and Date..."

    # Membersihkan terminal WSL
    clear

    # Loop untuk memperbarui tanggal dan waktu setiap detik
    while true; do
        # Ambil tanggal dan waktu saat ini
        current_date=$(date +"%A, %d %B %Y")
        current_time=$(date +"%T")

        # Bersihkan terminal dan tampilkan tanggal dan waktu
        clear
        echo -e "\033[1;32mTanggal:\033[0m $current_date"
        echo -e "\033[1;34mWaktu :\033[0m $current_time"

        # Tunggu 1 detik sebelum memperbarui
        sleep 1
    done
```
Sebelum fungsi dijalankan, terminal harus dibersihkan dulu dengan mengguanakn ```clear```, lalu fungsi akan melakukan perulangan while dan mengupdate setiap detiknya menggunakan ```sleep 1```.
Untuk outputnya seperti ini. Untuk menghentikan proses, cukup tekan ```ctrl + c``` pada keyboard.

https://github.com/user-attachments/assets/79b272f4-1328-470d-aa30-1b0108b82a1b
### D. Fungsi ```Money```
FUngsi ini diawali dengan ```clear``` lalu dilakuakn beberapa inisialisasi, seperti warna, posisi, kecepatan, colom dan baris. Untuk kodenya sebagai berikut
```
    clear

    # Daftar simbol mata uang
    currencies=("$" "€" "£" "¥" "¢" "₹" "₩" "₿" "₣")

    # Warna teks
    RED='\033[0;31m'
    GREEN='\033[0;32m'
    YELLOW='\033[0;33m'
    BLUE='\033[0;34m'
    CYAN='\033[0;36m'
    PURPLE='\033[0;35m'
    NC='\033[0m' # No Color

    # Inisialisasi array untuk menyimpan posisi dan kecepatan setiap kolom
    declare -a positions
    declare -a speeds

    # Jumlah kolom (sesuaikan dengan lebar terminal)
    cols=$(tput cols)
    rows=$(tput lines)

    # Inisialisasi posisi dan kecepatan awal untuk setiap kolom
    for ((i = 0; i < cols; i++)); do
        positions[$i]=$((RANDOM % rows))  # Posisi awal acak
        speeds[$i]=$((RANDOM % 3 + 1))    # Kecepatan acak (1-3)
    done
```

### E. Fungsi ```Brain Damage```
Untuk fungsi ini diawali dengan ```clear```, lalu inisialisasi warna teks sebagai berikut.
```
    RED='\033[0;31m'
    GREEN='\033[0;32m'
    YELLOW='\033[0;33m'
    BLUE='\033[0;34m'
    CYAN='\033[0;36m'
    PURPLE='\033[0;35m'
    NC='\033[0m
```
Setelah itu, fungsi while dijalankan dengan update setiap detiknya. Untuk kodenya seperti ini
```
    while true; do
        clear

        # Header
        echo -e "${PURPLE}PID     USER       CPU%   MEM%   COMMAND${NC}"
        echo "-------------------------------------------"

        # Ambil daftar proses dan tampilkan dengan warna acak setiap barisnya
        ps -eo pid,user,%cpu,%mem,comm --sort=-%cpu | head -n 11 | while read -r line; do
            # Pilih warna acak
            colors=("$RED" "$GREEN" "$YELLOW" "$BLUE" "$CYAN" "$PURPLE")
            color=${colors[$RANDOM % ${#colors[@]}]}

            # Tampilkan baris dengan warna
            echo -e "${color}${line}${NC}"
        done

        # Tunggu 1 detik sebelum update
        sleep 1
    done
````
Dimana pada ```ps -eo``` fungsi akan memanggil data berupa ```pid,user,%cpu,%mem,%comm```. Lalu untuk funggi ```head -n 11``` berfungsi untuk menampilkan output header + 10 baris proses terbesar. Untuk menghentikan proses ini cukup tekan ```ctrl + c``` pada keyboard.
Untuk output-nya sebagai berikut.
https://github.com/user-attachments/assets/d17ff3da-4beb-494f-9402-da47eea20dbc
