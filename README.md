## Week-1-Introduction-to-Data-Structures

This week focused on introducing basic data structure concepts and their practical implementation using the C programming language. Students learned about arrays, structs, and how to use them to store and process data efficiently. A hands-on coding task involved reading and analyzing athlete performance data from a file, determining each athlete's best result, and ranking them accordingly. This exercise emphasized data grouping, sorting, and conditional ranking logic.

---

### 🧩 Code Exercises

#### 🔹 Problem 1: *Athlete Long Jump Ranking*
*Description:*  
You are given a file named `jumpers.txt` containing a list of athlete names and their jump distances. Each athlete can have up to 3 jump records. Your task is to process this data and rank the athletes based on their best (farthest) jump.

If two or more athletes have the same best jump, they must share the same rank. The next rank should skip accordingly (e.g., 1, 2, 2, 4). The input is read from a file and the output should be printed in ranked order.

*Solution:*  
```c
// #include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_ATLET 8
#define MAX_NAMA 50
#define MAX_LOMPATAN 3

typedef struct {
    char nama[MAX_NAMA];
    double lompat_terbaik;
} Atlet;

int main() {
    Atlet atlet[MAX_ATLET];
    double lompatan[MAX_ATLET][MAX_LOMPATAN] = {0};
    char tempNama[MAX_NAMA];
    double tempLompatan;
    int jumlahAtlet = 0;

    FILE *file = fopen("jumpers.txt", "r");
    if (file == NULL) {
        printf("Error opening file\n");
        return 1;
    }

    while (fscanf(file, "%s %lf", tempNama, &tempLompatan) == 2) {
        int idx = -1;
        for (int i = 0; i < jumlahAtlet; i++) {
            if (strcmp(atlet[i].nama, tempNama) == 0) {
                idx = i;
                break;
            }
        }

        if (idx == -1 && jumlahAtlet < MAX_ATLET) {
            idx = jumlahAtlet++;
            strcpy(atlet[idx].nama, tempNama);
        }

        for (int j = 0; j < MAX_LOMPATAN; j++) {
            if (lompatan[idx][j] == 0) {
                lompatan[idx][j] = tempLompatan;
                break;
            }
        }
    }
    fclose(file);

    for (int i = 0; i < jumlahAtlet; i++) {
        double max_lompat = lompatan[i][0];
        for (int j = 1; j < MAX_LOMPATAN; j++) {
            if (lompatan[i][j] > max_lompat) {
                max_lompat = lompatan[i][j];
            }
        }
        atlet[i].lompat_terbaik = max_lompat;
    }

    for (int i = 0; i < jumlahAtlet - 1; i++) {
        for (int j = 0; j < jumlahAtlet - 1 - i; j++) {
            if (atlet[j].lompat_terbaik < atlet[j + 1].lompat_terbaik) {
                Atlet temp = atlet[j];
                atlet[j] = atlet[j + 1];
                atlet[j + 1] = temp;
            }
        }
    }

    printf("\nPERINGKAT ATLET\n");
    int peringkat = 1;
    for (int i = 0; i < jumlahAtlet; i++) {
        if (i > 0 && atlet[i].lompat_terbaik < atlet[i - 1].lompat_terbaik) {
            peringkat = i + 1;
        }
        printf("%d. %-3s - %.2f meter\n", peringkat, atlet[i].nama, atlet[i].lompat_terbaik);
    }

    return 0;
}

/* contoh 
file jumpers.txt:
Budi 5.2
Rina 6.0
Tono 4.9
Siti 5.7
Andi 6.2
Lina 5.5
Joko 4.8
Dewi 6.1
Budi 5.8
Rina 6.3
Tono 5.1
Siti 5.9
Andi 6.4
Lina 5.6
Joko 4.9
Dewi 6.2
Budi 5.5
Rina 6.1
Tono 5.0
Siti 6.0
Andi 6.3
Lina 5.8
Joko 5.0
Dewi 6.0

output:
PERINGKAT ATLET
1. Andi       - 6.40 meter
2. Rina       - 6.30 meter
3. Dewi       - 6.20 meter
4. Siti       - 6.00 meter
5. Budi       - 5.80 meter
5. Lina       - 5.80 meter
7. Tono       - 5.10 meter
8. Joko       - 5.00 meter

catatan sesuai yang disuruh pak Yudhi:
- jika lompatan terjauh sama maka ranking juga sama, contoh Budi dan Lina rankingnya sama 
karena lompatan terjauh sama, setelah itu ranking loncat ke 7.
*/
```

#### 🔹 Problem 2: *Bazaar Profit Report*
*Description:*  
Create a program to process the financial data of up to 20 kiosks participating in a bazaar. For each kiosk, the user inputs its name, revenue, and expenses.

*Solution:*  
```c
#include <stdio.h>
#include <string.h>

#define MAKS_KIOS 20

typedef struct {
    char nama[50];
    float pendapatan;
    float pengeluaran;
    float laba_bersih;
} Kios;

void bubbleSort(Kios kios[], int jumlah) {
    for (int i = 0; i < jumlah - 1; i++) {
        for (int j = 0; j < jumlah - 1 - i; j++) {
            if (kios[j].laba_bersih < kios[j + 1].laba_bersih) {
                Kios temp = kios[j];
                kios[j] = kios[j + 1];
                kios[j + 1] = temp;
            }
        }
    }
}

void tampilkan_hasil(Kios kios[], int jumlah, float total_laba) {
    printf("\nLaporan Keuangan Bazar:\n");
    for (int i = 0; i < jumlah; i++) {
        printf("%-2d. %-15s | Laba Bersih: %.2f\n", i + 1, kios[i].nama, kios[i].laba_bersih);
    }

    printf("\nTotal Kios: %d\n", jumlah);
    if (total_laba > 0) {
        printf("Total Laba Bazar: +%.2f\n", total_laba);
    } else if (total_laba < 0) {
        printf("Total Rugi Bazar: %.2f\n", total_laba);
    } else {
        printf("Bazar Impas (Tidak Untung/Tidak Rugi)\n");
    }

    printf("Kios dengan Laba Tertinggi:\n");
    float laba_tertinggi = kios[0].laba_bersih;
    for (int i = 0; i < jumlah; i++) {
        if (kios[i].laba_bersih == laba_tertinggi) {
            printf("- %s (%.2f)\n", kios[i].nama, kios[i].laba_bersih);
        } else {
            break;
        }
    }
}

int main() {
    Kios kios[MAKS_KIOS];
    int jumlah = 0;
    float total_laba = 0.0;

    printf("Masukkan data kios (Nama Pendapatan Pengeluaran). \nKetik 'xxxxxx' untuk mengakhiri:\n");

    while (jumlah < MAKS_KIOS) {
        char nama_kios[50];
        float pendapatan, pengeluaran;

        scanf("%s", nama_kios);
        if (strcmp(nama_kios, "xxxxxx") == 0) break;

        if (scanf("%f %f", &pendapatan, &pengeluaran) != 2) {
            printf("Input tidak valid, coba lagi.\n");
            continue;
        }

        strcpy(kios[jumlah].nama, nama_kios);
        kios[jumlah].pendapatan = pendapatan;
        kios[jumlah].pengeluaran = pengeluaran;
        kios[jumlah].laba_bersih = pendapatan - pengeluaran;
        total_laba += kios[jumlah].laba_bersih;

        jumlah++;
    }

    if (jumlah == 0) {
        printf("\nTidak ada data kios yang dimasukkan.\n");
        return 0;
    }

    bubbleSort(kios, jumlah);

    tampilkan_hasil(kios, jumlah, total_laba);

    return 0;
}

/* contoh
input:
Kios1 50000 20000
Kios2 60000 25000
Kios3 70000 30000
Kios4 40000 15000
Kios5 30000 20000
xxxxxx --> untuk mengakhiri input

output:
Laporan Keuangan Bazar:
1. Kios3           | Laba Bersih: 40000.00
2. Kios2           | Laba Bersih: 35000.00
3. Kios1           | Laba Bersih: 30000.00
4. Kios4           | Laba Bersih: 25000.00
5. Kios5           | Laba Bersih: 10000.00

Total Kios: 5
Total Laba Bazar: +140000.00
Kios dengan Laba Tertinggi:
- KiosC (40000.00)
*/
```

#### 🔹 Problem 3: *KTP Data Management System*
*Description:*  
Create a program in C that can be used to manage KTP (Indonesian ID card) data. The program should include features to:
- Add new KTP data.
- Search for data by NIK.
- Display all stored data.
- Delete data by NIK.

Each KTP data consists of several attributes, such as NIK, name, place and date of birth, gender, blood type, address, RT/RW, village, district, religion, marital status, occupation, nationality, and the validity of the KTP.

The program should continue to run interactively until the user chooses to exit (option `0`).

*Solution:*
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <conio.h>

#define MAX_DATA 100

typedef struct {
    char nik[20];
    char nama[50];
    char tempat_tgl_lahir[50];
    char jenis_kelamin[10];
    char gol_darah[5];
    char alamat[100];
    char rt_rw[10];
    char kel_desa[50];
    char kecamatan[50];
    char agama[20];
    char status_perkawinan[20];
    char pekerjaan[50];
    char kewarganegaraan[20];
    char berlaku_hingga[20];
} KTP;

KTP dataKTP[MAX_DATA];
int countData = 0;

void tambahData() {
    if (countData >= MAX_DATA) {
        printf("Data penuh! Tidak dapat menambah data lagi.\n");
        return;
    }

    printf("\nTambah Data (Masukkan sesuai format KTP asli mu)\n");
    printf("Masukkan NIK              : ");
    scanf(" %[^\n]", dataKTP[countData].nik);
    printf("Masukkan Nama             : ");
    scanf(" %[^\n]", dataKTP[countData].nama);
    printf("Masukkan Tempat/Tgl. Lahir: ");
    scanf(" %[^\n]", dataKTP[countData].tempat_tgl_lahir);
    printf("Masukkan Jenis Kelamin    : ");
    scanf(" %[^\n]", dataKTP[countData].jenis_kelamin);
    printf("Masukkan Gol. Darah       : ");
    scanf(" %[^\n]", dataKTP[countData].gol_darah);
    printf("Masukkan Alamat           : ");
    scanf(" %[^\n]", dataKTP[countData].alamat);
    printf("Masukkan RT/RW            : ");
    scanf(" %[^\n]", dataKTP[countData].rt_rw);
    printf("Masukkan Kel/Desa         : ");
    scanf(" %[^\n]", dataKTP[countData].kel_desa);
    printf("Masukkan Kecamatan        : ");
    scanf(" %[^\n]", dataKTP[countData].kecamatan);
    printf("Masukkan Agama            : ");
    scanf(" %[^\n]", dataKTP[countData].agama);
    printf("Masukkan Status Perkawinan: ");
    scanf(" %[^\n]", dataKTP[countData].status_perkawinan);
    printf("Masukkan Pekerjaan        : ");
    scanf(" %[^\n]", dataKTP[countData].pekerjaan);
    printf("Masukkan Kewarganegaraan  : ");
    scanf(" %[^\n]", dataKTP[countData].kewarganegaraan);
    printf("Masukkan Berlaku Hingga   : ");
    scanf(" %[^\n]", dataKTP[countData].berlaku_hingga);

    countData++;
    printf("Data berhasil ditambahkan!\n");
}

void tampilData() {
    if (countData == 0) {
        printf("Data masih kosong.\n");
        return;
    }
    printf("\nDaftar Data\n");
    for (int i = 0; i < countData; i++) {
        printf("\nData ke-%d\n", i + 1);
        printf("NIK              : %s\n", dataKTP[i].nik);
        printf("Nama             : %s\n", dataKTP[i].nama);
        printf("Tempat/Tgl. Lahir: %s\n", dataKTP[i].tempat_tgl_lahir);
        printf("Jenis Kelamin    : %s\n", dataKTP[i].jenis_kelamin);
        printf("Gol. Darah       : %s\n", dataKTP[i].gol_darah);
        printf("Alamat           : %s\n", dataKTP[i].alamat);
        printf("RT/RW            : %s\n", dataKTP[i].rt_rw);
        printf("Kel/Desa         : %s\n", dataKTP[i].kel_desa);
        printf("Kecamatan        : %s\n", dataKTP[i].kecamatan);
        printf("Agama            : %s\n", dataKTP[i].agama);
        printf("Status Perkawinan: %s\n", dataKTP[i].status_perkawinan);
        printf("Pekerjaan        : %s\n", dataKTP[i].pekerjaan);
        printf("Kewarganegaraan  : %s\n", dataKTP[i].kewarganegaraan);
        printf("Berlaku Hingga   : %s\n", dataKTP[i].berlaku_hingga);
    }
}

void cariData() {
    if (countData == 0) {
        printf("Data masih kosong.\n");
        return;
    }

    char cariNIK[20];
    printf("\nCari Data\n");
    printf("Masukkan NIK: ");
    scanf(" %[^\n]", cariNIK);

    int found = 0;
    for (int i = 0; i < countData; i++) {
        if (strcmp(dataKTP[i].nik, cariNIK) == 0) { //penting
            printf("\nData ditemukan:\n");
            printf("NIK              : %s\n", dataKTP[i].nik);
            printf("Nama             : %s\n", dataKTP[i].nama);
            printf("Tempat/Tgl. Lahir: %s\n", dataKTP[i].tempat_tgl_lahir);
            printf("Jenis Kelamin    : %s\n", dataKTP[i].jenis_kelamin);
            printf("Gol. Darah       : %s\n", dataKTP[i].gol_darah);
            printf("Alamat           : %s\n", dataKTP[i].alamat);
            printf("RT/RW            : %s\n", dataKTP[i].rt_rw);
            printf("Kel/Desa         : %s\n", dataKTP[i].kel_desa);
            printf("Kecamatan        : %s\n", dataKTP[i].kecamatan);
            printf("Agama            : %s\n", dataKTP[i].agama);
            printf("Status Perkawinan: %s\n", dataKTP[i].status_perkawinan);
            printf("Pekerjaan        : %s\n", dataKTP[i].pekerjaan);
            printf("Kewarganegaraan  : %s\n", dataKTP[i].kewarganegaraan);
            printf("Berlaku Hingga   : %s\n", dataKTP[i].berlaku_hingga);
            found = 1;
            break;
        }
    }
    if (!found) {
        printf("Data dengan NIK '%s' tidak ditemukan.\n", cariNIK);
    }
}

void hapusData() {
    if (countData == 0) {
        printf("Data masih kosong.\n");
        return;
    }

    char hapusNIK[20];
    printf("\nHapus Data\n");
    printf("Masukkan NIK yang akan dihapus: ");
    scanf(" %[^\n]", hapusNIK);

    int found = 0;
    for (int i = 0; i < countData; i++) {
        if (strcmp(dataKTP[i].nik, hapusNIK) == 0) {
            found = 1;
            for (int j = i; j < countData - 1; j++) {
                dataKTP[j] = dataKTP[j + 1];
            }
            countData--;
            printf("Data dengan NIK '%s' berhasil dihapus.\n", hapusNIK);
            break;
        }
    }

    if (!found) {
        printf("Data dengan NIK '%s' tidak ditemukan.\n", hapusNIK);
    }
}

int main() {
    int pilihan;

    do {
        printf("\nProgram Menu Data-data KTP Penduduk (Ketik 0 untuk akhiri)\n");
        printf("1. Tambah Data\n");
        printf("2. Cari Data\n");
        printf("3. Tampilkan Data\n");
        printf("4. Hapus Data\n");
        printf("Pilihan Anda: ");
        scanf("%d", &pilihan);

        if (pilihan == 1) {
            tambahData();
        } else if (pilihan == 2) {
            cariData();
        } else if (pilihan == 3) {
            tampilData();
        } else if (pilihan == 4) {
            hapusData();
        } else if (pilihan == 0) {
            printf("Terima kasih. Program berakhir.\n");
        } else {
            printf("Pilihan tidak valid! Program berakhir.\n");
            break;
        }
    } while (pilihan != 0);

    return 0;
}

/*
Tidak perlu contoh inputan karena program sudah saya buat gampang dimengerti.
 */
```
