---
title: N-QUEENS PROBLEM
date: 2025-05-20 08:00:00 +0800
categories: [DESAIN ANALISIS ALGORITMA, BACKTRACKING ALGORITHM]
tags: [daa, algorithm, backtracking]
---

![Desktop View](/assets/images/post/NQUEENS.png)

# Tantangan Ratu Catur: Memecahkan N-Queens Problem

## 👑 Pengantar: Konflik di Atas Papan Catur

Selamat datang di dunia **N-Queens Problem**, sebuah teka-teki klasik yang menguji logika dan kemampuan algoritmik kita!

**Masalah N-Queens** adalah sebuah tantangan fundamental dalam ilmu komputer dan matematika kombinatorik. Ini melibatkan penempatan **N buah ratu catur** pada sebuah papan catur berukuran $N \times N$, sedemikian rupa sehingga **tidak ada dua ratu yang saling menyerang**.

Ingat, dalam catur, ratu dapat bergerak secara:
* **Horizontal** (baris)
* **Vertikal** (kolom)
* **Diagonal** (kiri-atas, kanan-atas, kiri-bawah, kanan-bawah)

Jadi, solusi masalah ini harus memastikan bahwa tidak ada dua ratu yang berada pada baris, kolom, atau diagonal yang sama. Contoh paling terkenal adalah **8-Queens Problem**, yaitu menempatkan 8 ratu di papan $8 \times 8$.

---

## 🧠 Konsep Utama: Mencari Solusi Tanpa Konflik

N-Queens Problem bukan hanya tentang catur; ini adalah studi kasus sempurna untuk:

* **Menemukan Konfigurasi Optimal**: Tujuan utamanya adalah menemukan *semua* konfigurasi penempatan ratu yang memenuhi syarat (tidak saling menyerang).
* **Pengembangan Algoritma**: Ini adalah arena yang bagus untuk menguji dan mengembangkan berbagai algoritma pencarian dan optimasi, seperti **backtracking**, *Depth-First Search (DFS)*, *heuristic search*, atau bahkan *algoritma genetika*.
* **Melatih Logika Pemrograman**: Masalah ini sangat efektif untuk melatih logika pemrograman dan kemampuan pemecahan masalah, terutama dalam konteks **Constraint Satisfaction Problem (CSP)**, di mana kita harus memenuhi serangkaian batasan.

---

## 🧑‍💻 Backtracking: Strategi Uji Coba dan Mundur

### Apa itu Backtracking?

**Backtracking** adalah teknik penyelesaian masalah yang mencoba membangun solusi langkah demi langkah. Jika pada suatu langkah ditemukan bahwa jalur yang diambil tidak mungkin mengarah ke solusi yang valid (yaitu, menemui "jalan buntu"), algoritma akan "mundur" (*backtrack*) ke langkah sebelumnya dan mencoba pilihan yang berbeda. Ini adalah pendekatan *brute-force* yang sistematis namun dioptimalkan, karena solusi yang tidak valid akan dibuang secepat mungkin.

### Backtracking dalam N-Queens Problem

1.  **Pendekatan Rekursif dan Bertahap**:
    * Kita menempatkan ratu satu per satu, biasanya dimulai dari baris pertama.
    * Untuk setiap baris, kita mencoba meletakkan ratu di setiap kolom satu per satu.
    * Setiap kali menempatkan ratu, kita **memeriksa apakah posisi tersebut aman** (tidak diserang oleh ratu-ratu yang sudah ditempatkan di baris sebelumnya).
    * Jika aman, kita **lanjut ke baris berikutnya** (panggilan rekursif).
    * Jika tidak ada posisi yang aman di baris saat ini, atau jika panggilan rekursif dari baris berikutnya tidak menghasilkan solusi, kita **kembali ke baris sebelumnya** dan mencoba posisi lain. Inilah proses **backtracking**.

2.  **Pohon Pencarian Solusi**:
    * Setiap penempatan ratu mewakili satu simpul dalam pohon pencarian.
    * Jalur dari akar ke daun mewakili satu kemungkinan konfigurasi.
    * Jika sebuah jalur tidak bisa menghasilkan solusi yang valid, kita **memangkas (*prune*)** jalur itu dan mencoba cabang lain, menghindari eksplorasi yang tidak perlu.

3.  **Sifat Non-deterministik**:
    * Karena ada banyak kemungkinan posisi untuk setiap ratu, dan kita tidak tahu posisi yang benar sejak awal, kita harus mencoba semua kemungkinan secara sistematis. Ini menjadikan backtracking sebagai teknik yang sangat cocok untuk masalah ini.

---

## 📥 Input & 📤 Output

### Input

Biasanya, input untuk N-Queens Problem adalah satu **angka bulat positif $N$**, yang menyatakan:
* Ukuran papan catur ($N \times N$)
* Jumlah ratu yang harus ditempatkan

### Output

Output dapat berupa semua solusi valid (atau hanya satu solusi, tergantung kebutuhan), dengan format yang bervariasi:

1.  Sebagai **daftar posisi ratu per baris** (misal: `[2, 4, 1, 3]` untuk 4-Queens, artinya ratu di baris 0 kolom 2, baris 1 kolom 4, dst.).
2.  Sebagai **representasi papan visual** (seperti string 2D atau array karakter, dengan 'Q' untuk ratu dan '.' untuk kotak kosong).
3.  **Jumlah total solusi** yang ditemukan.

---

## 🧮 Langkah-langkah Penyelesaian (dengan Backtracking)

1.  **Pilih Keputusan (Decision Choice)**: Di setiap langkah (misalnya, di setiap baris), kita harus memutuskan di kolom mana ratu akan ditempatkan.
2.  **Batasan (Constraint Check)**: Sebelum menempatkan ratu di suatu posisi, periksa apakah posisi tersebut aman (tidak diserang oleh ratu yang sudah ditempatkan).
3.  **Rekursi (Recursive Exploration)**: Jika posisi aman, tempatkan ratu di sana dan panggil fungsi secara rekursif untuk baris berikutnya.
4.  **Backtrack (Kembali jika Dead End)**: Jika panggilan rekursif dari baris berikutnya tidak berhasil menemukan solusi (atau semua kolom di baris ini sudah dicoba dan tidak aman), "lepaskan" ratu dari posisi saat ini dan kembali ke baris sebelumnya untuk mencoba kolom lain.
5.  **Basis Kasus (Base Case)**: Jika kita berhasil menempatkan ratu di semua $N$ baris (yaitu, `row == N`), berarti kita telah menemukan satu solusi valid. Simpan solusi ini.

---

## 🧩 Contoh: Mencari Permutasi (Konsep Mirip Backtracking)

Meskipun contoh di bawah adalah Permutasi, ia sangat baik untuk menjelaskan konsep Backtracking yang juga digunakan di N-Queens:

**Problem: CARI SEMUA PERMUTASI DARI [1, 2, 3]**

**Langkah Solusi (Analogi Backtracking):**

1.  **Pilih angka pertama: 1**
    * _Subproblem_: Cari permutasi `[2, 3]` dari sisa angka.
        * (Ambil 2) $\rightarrow$ `[1, 2]`
            * _Subproblem_: Cari permutasi `[3]` dari sisa angka.
                * (Ambil 3) $\rightarrow$ `[1, 2, 3]` (Solusi ditemukan!)
                * _Backtrack_: Lepas 3 dari `[1, 2, 3]`. Kembali ke `[1, 2]`.
            * _Backtrack_: Lepas 2 dari `[1, 2]`. Kembali ke `[1]`.
        * (Ambil 3) $\rightarrow$ `[1, 3]`
            * _Subproblem_: Cari permutasi `[2]` dari sisa angka.
                * (Ambil 2) $\rightarrow$ `[1, 3, 2]` (Solusi ditemukan!)
                * _Backtrack_: Lepas 2 dari `[1, 3, 2]`. Kembali ke `[1, 3]`.
            * _Backtrack_: Lepas 3 dari `[1, 3]`. Kembali ke `[1]`.
    * _Backtrack_: Lepas 1 dari `[1]`. Kembali ke `[]`.

2.  **Pilih angka pertama: 2**
    * _Subproblem_: Cari permutasi `[1, 3]` dari sisa angka. $\rightarrow$ `[2, 1, 3]`, `[2, 3, 1]`
    * _Backtrack_: Kembali ke `[]`.

3.  **Pilih angka pertama: 3**
    * _Subproblem_: Cari permutasi `[1, 2]` dari sisa angka. $\rightarrow$ `[3, 1, 2]`, `[3, 2, 1]`

**Solusi Akhir**:
`[[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1]]`

---

## 💻 Contoh Kode (C++)

```cpp
#include <iostream>
#include <vector>
#include <string> // Diperlukan untuk std::string
using namespace std;

class NQueens {
private:
    int N; // Ukuran papan catur (N x N)
    vector<vector<string>> solutions; // Menyimpan semua solusi yang ditemukan

    // Fungsi untuk memeriksa apakah menempatkan ratu di (row, col) aman
    bool isSafe(int row, int col, vector<string>& board) {
        // 1. Cek kolom di atas (baris yang sudah ditempati)
        // Kita hanya perlu cek ke atas karena kita mengisi baris demi baris dari atas ke bawah
        for (int i = 0; i < row; i++) {
            if (board[i][col] == 'Q') {
                return false; // Ada ratu di kolom yang sama di baris atas
            }
        }

        // 2. Cek diagonal kiri atas
        // Iterasi dari posisi saat ini ke kiri atas
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] == 'Q') {
                return false; // Ada ratu di diagonal kiri atas
            }
        }

        // 3. Cek diagonal kanan atas
        // Iterasi dari posisi saat ini ke kanan atas
        for (int i = row - 1, j = col + 1; i >= 0 && j < N; i--, j++) {
            if (board[i][j] == 'Q') {
                return false; // Ada ratu di diagonal kanan atas
            }
        }

        return true; // Posisi (row, col) aman untuk menempatkan ratu
    }

    // Fungsi rekursif untuk menyelesaikan N-Queens
    // row: baris saat ini yang sedang kita coba tempatkan ratu
    // board: representasi papan catur saat ini
    void solve(int row, vector<string>& board) {
        // Basis Kasus: Jika kita berhasil menempatkan ratu di semua baris (row == N)
        if (row == N) {
            solutions.push_back(board); // Tambahkan konfigurasi papan saat ini ke daftar solusi
            return; // Kembali (berhasil menemukan solusi)
        }

        // Coba tempatkan ratu di setiap kolom di baris saat ini
        for (int col = 0; col < N; col++) {
            // Periksa apakah menempatkan ratu di (row, col) aman
            if (isSafe(row, col, board)) {
                board[row][col] = 'Q';     // Tempatkan ratu ('Q') di posisi ini
                solve(row + 1, board);     // Lanjut secara rekursif ke baris berikutnya
                board[row][col] = '.';     // Backtrack: Hapus ratu (ganti dengan '.')
                                           // untuk mencoba kolom lain di baris yang sama
            }
        }
    }

public:
    // Fungsi publik untuk memulai proses penyelesaian N-Queens
    void solveNQueens(int n) {
        N = n; // Set ukuran papan
        solutions.clear(); // Bersihkan solusi dari eksekusi sebelumnya

        // Inisialisasi papan catur kosong (semua '.' )
        vector<string> board(N, string(N, '.'));

        // Mulai proses rekursif dari baris 0
        solve(0, board);

        // Tampilkan semua solusi yang ditemukan
        cout << "Jumlah solusi untuk N = " << N << " adalah: " << solutions.size() << endl;
        int solutionCount = 1;
        for (const auto& sol : solutions) {
            cout << "Solusi " << solutionCount++ << ":" << endl;
            for (const string& row : sol) {
                cout << row << endl;
            }
            cout << endl; // Baris kosong antar solusi
        }
    }
};

int main() {
    int n;
    cout << "Masukkan nilai N (jumlah ratu): ";
    cin >> n;

    // Pastikan N positif
    if (n <= 0) {
        cout << "N harus angka positif." << endl;
        return 1;
    }
    
    NQueens solver; // Buat objek solver
    solver.solveNQueens(n); // Jalankan algoritma

    return 0;
}
```

---

## 📚 Analisis Kompleksitas

### Kompleksitas Waktu

* **Pencarian**: Dalam skenario terburuk, algoritma *backtracking* akan menjelajahi setiap kemungkinan penempatan ratu. Untuk menempatkan $N$ ratu, ada $N$ pilihan untuk ratu pertama, $N-1$ untuk ratu kedua (jika kita mengabaikan batasan diagonal), dan seterusnya, mengarah ke **$N!$** (N faktorial) kemungkinan permutasi.
* **Pengecekan Keamanan**: Pada setiap langkah (menempatkan ratu di baris `row`, kolom `col`), fungsi `isSafe` memeriksa baris, kolom, dan diagonal di atas posisi tersebut. Pemeriksaan ini memakan waktu $O(N)$ (karena paling banyak memeriksa $N$ sel).
* **Total**: Meskipun banyak cabang dipangkas oleh `isSafe`, pada dasarnya kompleksitas waktu **secara teoretis mendekati $O(N!)$**, atau lebih tepatnya, sekitar $O(N^2 \cdot N!)$ atau $O(N \cdot N!)$ tergantung pada implementasi `isSafe`. Ini adalah kompleksitas **eksponensial faktorial**, yang tumbuh sangat cepat.

### Kompleksitas Ruang

1.  **Papan Catur (`board[N][N]`)**: Disimpan sebagai `vector<string>` atau array 2D, memerlukan $O(N^2)$ ruang untuk merepresentasikan papan.
2.  **Stack Rekursi**: Kedalaman maksimum rekursi adalah $N$ (karena kita menempatkan satu ratu per baris). Setiap panggilan rekursif menambah *frame* ke *stack*, yang menyimpan variabel lokal. Ini berkontribusi $O(N)$ ruang.
3.  **Penyimpanan Semua Solusi (`solutions`)**: Jika kita menyimpan *semua* solusi yang ditemukan, dan ada $S$ solusi, maka total ruang yang dibutuhkan adalah $O(S \times N^2)$, karena setiap solusi adalah papan $N \times N$. Nilai $S$ sendiri bisa sangat besar untuk $N$ yang lebih besar.

**Total kompleksitas ruang**: $O(N^2)$ (untuk satu papan dan stack rekursi) jika kita tidak menyimpan semua solusi. Jika semua solusi disimpan, bisa menjadi $O(S \cdot N^2)$.

---

## 🌟 Aplikasi Dunia Nyata

Meskipun terlihat seperti teka-teki catur, konsep di balik N-Queens punya relevansi di berbagai bidang:

* **Penjadwalan Tugas**: Ketika ada tugas-tugas yang tidak boleh tumpang tindih atau menggunakan sumber daya yang sama pada waktu yang sama, seperti penjadwalan kelas atau jadwal produksi pabrik.
* **Penempatan Modul dalam Sirkuit Elektronik**: Mengatur komponen pada *chip* atau papan sirkuit agar tidak ada *interferensi* atau *koneksi* yang tidak diinginkan.
* **Pengalokasian Sumber Daya Eksklusif**: Dalam sistem komputasi atau manufaktur, menentukan bagaimana mengalokasikan sumber daya yang hanya bisa digunakan oleh satu proses/mesin pada satu waktu.
* **Penempatan Karyawan di Ruang Kerja**: Merencanakan tata letak kantor untuk menghindari gangguan atau memaksimalkan kolaborasi (meskipun dengan batasan yang berbeda dari ratu).

---

## 💪 Kelebihan dan Kekurangan

### Kelebihan

* **Studi Kasus Algoritma Klasik**: Ini adalah salah satu contoh terbaik untuk memahami dan mengimplementasikan algoritma *backtracking*.
* **Mengasah Kemampuan Pemecahan Masalah**: Memaksa Anda berpikir logis tentang batasan dan cara menelusuri ruang solusi.
* **Mudah Dipahami Secara Visual**: Konsepnya intuitif karena terkait dengan papan catur yang familier.
* **Basis Masalah Kompleks**: Prinsip *backtracking* yang dipelajari dari N-Queens dapat diterapkan ke banyak masalah lain yang lebih rumit.
* **Benchmarking Algoritma**: Sering digunakan sebagai *benchmark* untuk menguji kinerja berbagai teknik optimasi atau bahasa pemrograman.

### Kekurangan

* **Kompleksitas Eksponensial**: Solusi akan sangat lambat seiring bertambahnya $N$. Ini tidak praktis untuk $N$ yang sangat besar.
* **Skalabilitas Terbatas**: Karena kompleksitas eksponensial faktorial, N-Queens tidak skalabel untuk $N > \approx 15-20$ dalam waktu yang masuk akal.
* **Masalah "Artifisial"**: Meskipun konsepnya relevan, masalah itu sendiri agak spesifik dan tidak langsung mencerminkan masalah dunia nyata tanpa abstraksi.
* **Terlalu Spesifik**: Solusi *greedy* tidak akan bekerja, dan hanya pendekatan yang menjelajahi ruang solusi secara sistematis (seperti *backtracking*) yang efektif.

---

## 🏁 Kesimpulan

**Masalah N-Queens** memberikan pemahaman yang mendalam tentang bagaimana algoritma bekerja dalam menyelesaikan masalah pencarian dan optimasi melalui pendekatan **backtracking**. Dengan mempelajari N-Queens, kita tidak hanya memahami cara kerja rekursi dan strategi pemangkasan ruang pencarian, tetapi juga mampu menerapkan logika algoritmik dalam konteks permasalahan yang memiliki banyak kemungkinan solusi.

Walaupun bersifat teoritis dan memiliki keterbatasan skalabilitas karena kompleksitasnya yang tinggi, N-Queens sangat berguna sebagai sarana latihan untuk membangun fondasi berpikir komputasional yang kuat, serta membuka wawasan terhadap masalah-masalah sejenis yang lebih kompleks dalam dunia nyata, seperti penjadwalan, alokasi sumber daya, dan pemrograman kendala.

---