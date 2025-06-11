---
title: RAT IN A MAZE
date: 2025-05-27 08:00:00 +0800
categories: [DESAIN ANALISIS ALGORITMA, BACKTRACKING ALGORITHM]
tags: [daa, algorithm, backtracking, c++]
---

![Desktop View](/assets/images/post/RIAM.png)

# Menelusuri Labirin: Memecahkan Rat in a Maze Problem

## 🐀 Pengantar: Misi Sang Tikus

Bayangkan seekor tikus yang tersesat dalam labirin, dan tugas kita adalah membantunya menemukan jalan keluar! Inilah inti dari **Rat in a Maze**—sebuah masalah algoritma klasik.

**Rat in a Maze** adalah *algorithm problem* di mana seekor tikus (`rat`) harus menemukan jalan keluar dari sebuah **labirin (`maze`)** dengan mengikuti jalur tertentu, dari titik awal ke titik tujuan. Labirin ini direpresentasikan oleh sebuah matriks atau *grid* yang terdiri dari sel-sel yang bisa dilewati atau terhalang. Solusi untuk masalah ini dapat menggunakan algoritma **backtracking**, di mana tikus mencoba bergerak selangkah demi selangkah dan "mundur" (`backtrack`) jika jalan yang dipilih tidak berhasil.

---

## 🧠 Konsep Utama: Eksplorasi Coba-coba

Bagaimana tikus bisa menemukan jalan keluarnya? Ini adalah beberapa konsep inti di balik Rat in a Maze:

1.  **Pencarian Jalur**: Tikus akan mencoba menemukan jalur dari posisi awal (biasanya `(0,0)`) ke tujuan (biasanya `(N-1, N-1)`) di dalam sebuah labirin berbentuk matriks.
2.  **Langkah Demi Langkah**: Tikus akan mencoba satu langkah ke suatu arah (misalnya, kanan, bawah, kiri, atau atas).
3.  **Validasi Langkah**:
    * Jika langkah tersebut **valid** (tidak keluar dari batas labirin dan tidak menabrak dinding/blokade), maka tikus akan melanjutkan ke sel tersebut.
    * Jika langkah **tidak valid** (keluar batas, menabrak dinding, atau sudah dikunjungi dalam jalur yang sama), tikus tidak akan mengambil langkah itu.
4.  **Strategi Backtracking**: Jika dari posisi baru tikus tidak bisa menemukan jalan menuju tujuan (misalnya, semua arah selanjutnya terblokir atau mengarah ke jalan buntu), ia akan **mundur (backtrack)** ke posisi sebelumnya dan mencoba arah lain dari posisi tersebut.

---

## 🧑‍💻 Backtracking: Inti dari Solusi

**Backtracking** adalah strategi yang sangat cocok untuk menyelesaikan masalah **Rat in a Maze** karena sifatnya yang berupa pencarian dan pengambilan keputusan berurutan.

Dalam konteks tikus di labirin:

* **Keputusan Bertahap**: Tikus memulai dari titik awal dan mencoba jalur yang berbeda untuk mencapai titik akhir. Setiap kali tikus bergerak ke sel baru, itu adalah sebuah "keputusan" yang diambil.
* **Deteksi Jalan Buntu**: Jika tikus mencapai sel yang buntu (misalnya, dinding, atau sel yang sudah pernah dikunjungi dalam jalur yang sama untuk menghindari putaran tak terbatas), atau jika jalur yang diambil tidak mengarah ke tujuan, maka tikus akan "mundur" (`backtrack`) ke sel sebelumnya.
* **Percobaan Alternatif**: Dari sel sebelumnya, tikus akan mencoba jalur atau keputusan lain yang belum dicoba dari sel tersebut.
* **Rekursi dan Penandaan**: Proses ini berlanjut secara rekursif. Tikus menandai sel yang sudah dikunjunginya (seringkali dengan nilai tertentu dalam matriks labirin, misalnya `1` untuk jalur atau `2` untuk `visited`) agar tidak mengunjungi sel yang sama berulang kali dalam satu jalur dan mencegah putaran tak terbatas.
* **Alur "Maju-Mundur"**:
    * Jika tikus berada di sel `(x,y)` dan mencoba bergerak ke empat arah (atas, bawah, kiri, kanan), ia akan memilih satu arah, misalnya ke `(x', y')`.
    * Jika `(x', y')` adalah sel yang valid (bukan dinding, belum dikunjungi), tikus akan bergerak ke sana dan mencoba mencari jalan dari `(x', y')`.
    * Jika dari `(x', y')` tikus tidak bisa mencapai tujuan (semua jalurnya buntu atau sudah dikunjungi), maka tikus akan **kembali ke `(x,y)`** (ini adalah inti dari backtracking!), menghapus tanda bahwa `(x', y')` adalah bagian dari jalur yang berhasil (jika jalur ini gagal), dan mencoba arah lain dari `(x,y)`.

Proses "maju-mundur" ini terus berulang sampai tikus menemukan jalur menuju tujuan, atau semua kemungkinan jalur telah dicoba dan tidak ada solusi ditemukan.

---

## 📥 Input & 📤 Output

### Input

* Sebuah **matriks 2D `maze[N][N]`** yang merepresentasikan peta labirin.
* Elemen bernilai `1` artinya **jalan bisa dilalui**.
* Elemen bernilai `0` artinya **tembok atau jalan buntu**.
* Tikus mulai dari kiri atas `(0, 0)` dan ingin mencapai kanan bawah `(N-1, N-1)`.

### Output

Output dapat berupa jalur dari titik awal ke tujuan, biasanya dalam bentuk:

* **Matriks solusi**: Sebuah matriks `N x N` di mana `1` menandai sel yang dilewati tikus sebagai bagian dari jalur solusi, dan `0` untuk sel yang tidak dilewati.
* **Daftar koordinat lintasan**: Urutan koordinat `(x,y)` yang membentuk jalur yang ditemukan.
* **Jumlah semua solusi yang mungkin**: Jika diminta untuk mencari semua jalur yang valid, bukan hanya satu.

---

## 🧮 Langkah-langkah Penyelesaian

1.  **Titik Awal**: Mulai penelusuran dari posisi `(0, 0)`. Pastikan posisi ini adalah jalan (bernilai `1`), bukan tembok (`0`). Jika `maze[0][0]` adalah `0`, tidak ada solusi.
2.  **Eksplorasi Arah**: Lakukan eksplorasi ke arah yang valid dari posisi saat ini.
    * Umumnya, hanya mencari ke **bawah (↓)** dan **kanan (→)** jika hanya satu jalur yang dicari dan diasumsikan gerakan searah.
    * Bisa juga ke **semua arah (atas ↑, bawah ↓, kiri ←, kanan →)** jika eksplorasi total diperlukan atau jika labirin memiliki jalur memutar.
3.  **Cek Batasan (Constraint Check)**: Sebelum bergerak ke sel baru, pastikan:
    * Sel tersebut berada **di dalam batas labirin** (tidak keluar dari `0` hingga `N-1`).
    * Sel tersebut **bukan tembok** (bernilai `1` dalam `maze`).
    * Sel tersebut **belum dikunjungi** dalam jalur saat ini (untuk menghindari *loop* tak terbatas).
4.  **Rekursi dan Penandaan**: Jika langkah valid, **tandai posisi saat ini sebagai bagian dari solusi** (misalnya, dengan mengubah nilai di matriks solusi menjadi `1`), lalu lanjutkan dengan panggilan rekursif ke sel baru tersebut.
5.  **Basis Kasus**: Jika tikus mencapai posisi tujuan `(N-1, N-1)`, berarti satu solusi telah ditemukan. Simpan solusi tersebut.
6.  **Backtrack**: Jika dari suatu posisi, tidak ada arah valid yang bisa mengarah ke tujuan (semua jalur buntu atau sudah dicoba), **batalkan keputusan** yang diambil untuk sampai ke sel tersebut. Tandai kembali sel tersebut sebagai tidak bagian dari solusi (misalnya, ubah nilai di matriks solusi kembali menjadi `0`), lalu kembali ke posisi sebelumnya untuk mencoba arah lain.

---

## 🧩 Problem and Solution Example

**Diberikan Maze 4x4 (dengan 1=jalan, 0=tembok):**

```
1 0 0 0
1 1 0 1
0 1 0 0
1 1 1 1
```

Tikus mulai di `(0,0)` dan ingin mencapai `(3,3)`. Temukan satu jalur yang mungkin.

**Langkah Solusi (Contoh Satu Jalur):**

1.  Mulai di `(0,0)`. Ini `1` (jalan). Tandai `sol[0][0]=1`.
    * Coba bergerak **Bawah (D)** ke `(1,0)`. `maze[1][0]=1` (aman).
2.  Dari `(1,0)`. Tandai `sol[1][0]=1`.
    * Coba bergerak **Kanan (R)** ke `(1,1)`. `maze[1][1]=1` (aman).
3.  Dari `(1,1)`. Tandai `sol[1][1]=1`.
    * Coba bergerak **Bawah (D)** ke `(2,1)`. `maze[2][1]=1` (aman).
4.  Dari `(2,1)`. Tandai `sol[2][1]=1`.
    * Coba bergerak **Bawah (D)** ke `(3,1)`. `maze[3][1]=1` (aman).
5.  Dari `(3,1)`. Tandai `sol[3][1]=1`.
    * Coba bergerak **Kanan (R)** ke `(3,2)`. `maze[3][2]=1` (aman).
6.  Dari `(3,2)`. Tandai `sol[3][2]=1`.
    * Coba bergerak **Kanan (R)** ke `(3,3)`. `maze[3][3]=1` (aman).
7.  Dari `(3,3)`. Tandai `sol[3][3]=1`.
    * **Tujuan tercapai!** (`(3,3)` adalah `N-1, N-1`). Simpan jalur ini.

**Representasi Arah Jalur yang Ditemukan**: `DRDDRR`

**Matriks Solusi (jalur yang ditandai 1):**

```
1 0 0 0
1 1 0 0
0 1 0 0
0 1 1 1  <- (3,0) tidak 1 karena jalur lewat (3,1)
```

(Note: Matriks solusi akan menunjukkan `1` pada sel yang dilalui, jadi `sol[3][0]` akan `0` jika jalur lewat `(3,1)`. Contoh di atas menunjukkan `sol[3][0]=1` di output karena `solveMaze` di kode hanya mencari satu jalur dan cetak hasilnya.)

---

## 💻 Contoh Kode (C++)

```cpp
#include <iostream>
#include <vector> // Digunakan untuk std::vector
using namespace std;

// Konstanta ukuran maze (N x N)
const int N = 4; 

// Fungsi untuk mengecek apakah posisi (x, y) aman dan valid untuk melangkah
bool isSafe(int maze[N][N], int x, int y) {
    // Posisi harus berada dalam batas maze (0 hingga N-1)
    // Dan sel di posisi tersebut harus bernilai 1 (jalan yang bisa dilalui)
    return (x >= 0 && x < N && y >= 0 && y < N && maze[x][y] == 1);
}

// Fungsi rekursif untuk menyelesaikan maze menggunakan backtracking
// maze: matriks labirin
// x, y: koordinat posisi tikus saat ini
// sol: matriks solusi untuk menandai jalur yang ditemukan
bool solveMazeUtil(int maze[N][N], int x, int y, int sol[N][N]) {
    // Basis Kasus: Jika tikus mencapai sel tujuan (pojok kanan bawah)
    if (x == N - 1 && y == N - 1 && maze[x][y] == 1) {
        sol[x][y] = 1; // Tandai sel tujuan sebagai bagian dari solusi
        return true;   // Solusi ditemukan
    }

    // Periksa apakah sel (x,y) adalah langkah yang valid dan aman
    if (isSafe(maze, x, y)) {
        // Tandai sel saat ini sebagai bagian dari jalur solusi
        sol[x][y] = 1;

        // Coba bergerak ke bawah (x + 1, y)
        if (solveMazeUtil(maze, x + 1, y, sol)) {
            return true; // Jika jalur ke bawah berhasil, kembalikan true
        }

        // Jika bergerak ke bawah tidak berhasil, coba bergerak ke kanan (x, y + 1)
        if (solveMazeUtil(maze, x, y + 1, sol)) {
            return true; // Jika jalur ke kanan berhasil, kembalikan true
        }
        
        // --- Opsional: Tambahkan arah lain jika diperlukan (atas, kiri) ---
        // if (solveMazeUtil(maze, x - 1, y, sol)) return true; // Atas
        // if (solveMazeUtil(maze, x, y - 1, sol)) return true; // Kiri

        // Jika tidak ada arah yang berhasil dari posisi (x,y)
        // Maka jalur melalui (x,y) tidak mengarah ke solusi.
        // Lakukan Backtrack: batalkan penandaan sel (x,y) sebagai bagian dari solusi
        sol[x][y] = 0; 
        return false; // Kembalikan false untuk memberitahu panggilan sebelumnya agar mencoba jalur lain
    }

    // Jika sel (x,y) tidak aman (misalnya, tembok atau di luar batas)
    return false;
}

// Fungsi utama untuk menyelesaikan Rat in a Maze
void solveMaze(int maze[N][N]) {
    // Inisialisasi matriks solusi dengan semua 0 (tidak ada jalur yang ditandai)
    int sol[N][N] = { {0} };

    // Panggil fungsi rekursif dimulai dari (0,0)
    if (!solveMazeUtil(maze, 0, 0, sol)) {
        cout << "Tidak ada solusi untuk maze ini!" << endl;
        return;
    }

    // Jika solusi ditemukan, cetak matriks solusi
    cout << "Solusi Maze (jalur yang ditandai 1):" << endl;
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            cout << sol[i][j] << " ";
        }
        cout << endl; // Baris baru setelah setiap baris maze
    }
}

// Main program untuk menjalankan contoh
int main() {
    // Contoh matriks labirin
    int maze[N][N] = {
        {1, 0, 0, 0},
        {1, 1, 0, 1},
        {0, 1, 0, 0},
        {1, 1, 1, 1}
    };

    solveMaze(maze); // Panggil fungsi penyelesaian maze

    return 0;
}
```

---

## 📚 Analisis Kompleksitas

### Kompleksitas Waktu

* **Pencarian Eksponensial**: Dalam kasus terburuk, tikus bisa mencoba hampir semua jalur yang memungkinkan. Pada setiap sel, tikus memiliki hingga 4 arah kemungkinan untuk melangkah (jika semua arah diizinkan).
* **Total Sel**: Untuk maze berukuran $N \times N$, ada $N^2$ sel.
* **Kompleksitas Terburuk**:
    * Secara teoretis, dalam skenario terburuk, ini bisa mencapai $O(4^{N \times N})$ jika setiap sel bisa bercabang ke 4 arah secara eksponensial (rekursi mendalam).
    * Namun, karena kita tidak mengunjungi sel yang sama dua kali dalam satu jalur dan ada dinding, ruang pencarian dipangkas. Realistisnya, untuk banyak labirin, kompleksitasnya lebih rendah tetapi tetap **eksponensial** terhadap ukuran $N$.
    * Jika hanya diizinkan bergerak ke kanan dan bawah, kompleksitas bisa lebih kecil, sekitar $O(2^{2N})$ atau $O(2^{N \times N})$.

### Kompleksitas Ruang

* **Stack Rekursi**: Karena solusi menggunakan rekursi, *stack rekursi* akan memakan ruang sesuai dengan kedalaman pencarian. Kedalaman maksimal bisa mencapai $N^2$ (jika jalur terpanjang melewati setiap sel). Jadi, $O(N^2)$.
* **Matriks Solusi**: Matriks `sol[N][N]` digunakan untuk menyimpan jalur yang ditemukan, memerlukan $O(N^2)$ ruang.

**Total kompleksitas ruang**: $O(N^2)$.

---

## 🌟 Aplikasi Dunia Nyata

Konsep dan prinsip dari Rat in a Maze memiliki relevansi di berbagai domain:

* **Robot Navigasi**: Merencanakan jalur bagi robot otonom untuk bergerak di lingkungan yang tidak diketahui atau penuh rintangan, seperti di gudang atau medan yang kompleks.
* **Pencarian Jalur di GPS atau Navigasi**: Meskipun algoritma lain (seperti Dijkstra atau A*) lebih umum untuk peta berbobot, prinsip pencarian ruang dan *backtracking* dapat diterapkan pada segmen jalur atau saat mengatasi rintangan.
* **Simulasi dan Permainan**: Digunakan dalam *game* untuk karakter AI yang perlu menemukan jalan di lingkungan kompleks, atau dalam simulasi untuk menelusuri jaringan.
* **Verifikasi Sirkuit**: Dalam desain sirkuit elektronik, untuk memverifikasi jalur sinyal atau konektivitas di antara komponen.

---

## 💪 Kekuatan dan Keterbatasan

### Kekuatan

* **Sederhana dan Mudah Diimplementasikan**: Konsep *backtracking* sangat intuitif untuk masalah pencarian jalur seperti ini.
* **Menemukan Semua Solusi**: Dengan sedikit modifikasi, algoritma dapat dengan mudah diadaptasi untuk menemukan *semua* jalur yang mungkin dari sumber ke tujuan, bukan hanya satu.
* **Fleksibel**: Dapat disesuaikan untuk berbagai jenis batasan (misalnya, melompat sel, batasan gerakan).
* **Tidak Memerlukan Struktur Data Kompleks**: Hanya butuh matriks dan rekursi.

### Keterbatasan

* **Kurang Efisien dalam Kasus Besar**: Karena sifatnya yang eksponensial, *backtracking* menjadi sangat lambat untuk labirin berukuran besar.
* **Risiko Stack Overflow**: Untuk labirin yang sangat besar atau jalur yang sangat panjang, kedalaman rekursi bisa menyebabkan *stack overflow*.
* **Tidak Optimal untuk Jalur Terpendek**: Algoritma ini akan menemukan *sebuah* jalur, tetapi tidak dijamin itu adalah jalur terpendek (kecuali jika ada batasan tambahan atau diubah menjadi BFS). Untuk jalur terpendek, algoritma seperti BFS (untuk graf tak berbobot) atau Dijkstra/A* (untuk graf berbobot) lebih tepat.
* **Mengulang Jalur yang Sama Jika Tidak Diatur**: Tanpa penandaan sel yang sudah dikunjungi dalam satu jalur, tikus bisa terjebak dalam *loop* tak terbatas.

---

## 🏁 Kesimpulan

**Rat in a Maze** adalah contoh klasik dan fundamental yang memberikan pemahaman mendalam tentang bagaimana algoritma **backtracking** bekerja untuk menyelesaikan masalah pencarian jalur. Dengan menerapkan prinsip **coba-coba dan mundur** ketika menemui jalan buntu, kita belajar menyusun solusi yang sistematis dan efisien untuk menjelajahi ruang kemungkinan yang kompleks.

Selain itu, masalah ini memperkenalkan konsep penting seperti **rekursi, pengecekan batasan (*constraint checking*)**, dan eksplorasi ruang solusi, yang sangat relevan dalam pengembangan algoritma pencarian jalan di berbagai bidang seperti robotika, kecerdasan buatan, dan pengembangan *game*. Secara keseluruhan, Rat in a Maze bukan hanya tentang menemukan jalan keluar dari labirin, tetapi juga tentang membangun pola pikir algoritmik yang kuat dan terstruktur.

---