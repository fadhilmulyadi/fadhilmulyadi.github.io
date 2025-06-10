---
title: DEPTH FIRST SEARCH
date: 2025-05-27 08:00:00 +0800
categories: [DESAIN ANALISIS ALGORITMA, BACKTRACKING ALGORITHM]
tags: [daa, algorithm, backtracking, dfs, c++]
---

![Desktop View](/assets/lib/images/post/DFS.png)

# Menyelam Lebih Dalam: Memahami Depth-First Search (DFS)

## 📶 Pengantar: Eksplorasi Hingga Titik Terdalam

Pernahkah Anda tersesat dalam pikiran, menjelajahi satu ide secara mendalam sebelum beralih ke yang lain? Itulah gambaran dari **Depth-First Search (DFS)**, sebuah algoritma pencarian yang kuat!

**Depth-First Search (DFS)** adalah algoritma fundamental untuk menjelajahi **graf** atau **pohon**. Berbeda dengan BFS yang menjelajahi secara berlapis, DFS bekerja dengan menjelajahi satu cabang **sedalam mungkin** sebelum "mundur" (*backtrack*) dan melanjutkan ke cabang berikutnya. Ini seperti menyusuri lorong panjang di dalam gua sampai menemukan jalan buntu atau keluar, baru kemudian kembali dan mencoba lorong lain.

---

## 🧠 Konsep Utama: Stack dan Penjelajahan Vertikal

DFS memiliki ciri khas yang membedakannya:

1.  **Struktur Data Kunci**: DFS menggunakan struktur data **stack** (baik secara eksplisit maupun implisit melalui rekursi). Ini berarti simpul terakhir yang ditambahkan ke "daftar tunggu" akan menjadi yang pertama dieksplorasi.
2.  **Mekanisme Penjelajahan**:
    * Dimulai dari **simpul akar/awal**.
    * Telusuri satu jalur **sedalam mungkin** hingga mencapai simpul daun (tidak punya anak) atau jalan buntu.
    * Jika jalan buntu, **mundur (backtrack)** ke simpul sebelumnya yang masih punya cabang yang belum dieksplorasi.
    * Lanjutkan penjelajahan dari cabang baru tersebut.
3.  **Sifat Traversal**: DFS adalah algoritma *depth-wise traversal*. Ini berarti ia akan mencoba menjelajahi satu cabang sepenuhnya hingga ke "kedalaman" maksimum sebelum mencoba cabang lain.

---

## 🧑‍💻 Backtracking dalam Depth-First Search (DFS)

Konsep **backtracking** adalah inti dari DFS. Ketika DFS digunakan untuk memecahkan masalah seperti pencarian jalur, kombinatorial, atau eksplorasi ruang solusi (misalnya, dalam Sudoku, N-Queens, atau Rat in a Maze), *backtracking* muncul secara alami.

* **Penggunaan Stack**: DFS bekerja dengan tumpukan (stack). Saat sebuah simpul dikunjungi, ia didorong ke stack. Tetangga yang belum dikunjungi akan terus didorong ke stack dan dieksplorasi.
* **Mencapai Jalan Buntu**: Ketika DFS mencapai simpul yang tidak memiliki tetangga yang belum dikunjungi (jalan buntu), atau tidak dapat melangkah lebih jauh sesuai batasan masalah, ia akan **pop** simpul tersebut dari stack. Ini adalah operasi "mundur" atau *backtrack*.
* **Mencoba Pilihan Lain**: Setelah mundur, algoritma akan melanjutkan eksplorasi dari simpul yang sekarang berada di puncak stack, mencoba jalur lain yang belum dieksplorasi dari simpul tersebut.

Proses ini terus berulang sampai seluruh graf (atau bagian yang relevan) telah dieksplorasi atau solusi ditemukan.

---

## 📥 Input & 📤 Output

### Input

Untuk menjalankan DFS, kita memerlukan:

* **Representasi Graf**:
    * **Matriks Adjacency**: Untuk graf kecil atau padat.
    * **Daftar Adjacency (Adjacency List)**: Lebih efisien untuk graf besar dan jarang (*sparse*).
    * **Matriks 2D**: Untuk kasus seperti pencarian jalur di *maze* atau *grid*.
* **Titik Awal (Source Node)**: Simpul dari mana penelusuran akan dimulai.
* **Titik Tujuan (opsional)**: Jika ingin menemukan jalur dari *source* ke *target*.

### Output

Tergantung pada tujuan DFS, output bisa berupa:

* **Urutan Simpul yang Dikunjungi**: Daftar simpul yang dijelajahi selama proses pencarian, dalam urutan DFS.
* **Jalur dari Simpul Awal ke Tujuan**: Jika diminta dan jalur ditemukan.
* **Status Pencapaian**: (Boolean) Apakah simpul tujuan ditemukan atau tidak.

---

## 🧮 Langkah-langkah Penyelesaian DFS

1.  **Mulai**: Pilih **simpul awal (start node)**.
2.  **Tandai**: Tandai simpul ini sebagai **dikunjungi**.
3.  **Rekursi/Stack**: Lakukan operasi pada simpul ini (misalnya, tambahkan ke daftar urutan kunjungan).
4.  **Eksplorasi Tetangga**: Untuk **setiap tetangga** dari simpul saat ini:
    * Jika tetangga tersebut **belum dikunjungi**:
        * Lakukan panggilan **DFS secara rekursif** ke tetangga tersebut.
5.  **Backtrack**: Jika dari simpul saat ini, semua tetangga yang valid sudah dieksplorasi atau tidak ada jalur lebih lanjut (jalan buntu), fungsi rekursif akan selesai, dan kontrol akan kembali ke panggilan sebelumnya (inilah proses *backtracking*).
6.  **Berhenti**: Proses berlanjut hingga semua simpul yang terhubung telah dieksplorasi atau simpul tujuan ditemukan.

---

## 🧩 Problem and Solution Example: Maze Solving

**Contoh Maze (4x4):**

```
1 1 0 1
0 1 0 1
1 1 1 1
0 0 1 1
```

* `1`: Bisa dilalui
* `0`: Halangan

**Start**: `(0,0)`
**Goal**: `(3,3)`

**Langkah Solusi (Contoh Satu Jalur DFS):**

1.  **Masuk (0,0)**. Tandai visited. Tambah ke jalur.
2.  Dari (0,0), coba arah **kanan (R)** ke `(0,1)`. `isSafe` = true.
3.  **Masuk (0,1)**. Tandai visited. Tambah ke jalur.
4.  Dari (0,1), coba arah **bawah (D)** ke `(1,1)`. `isSafe` = true.
5.  **Masuk (1,1)**. Tandai visited. Tambah ke jalur.
6.  Dari (1,1), coba arah **bawah (D)** ke `(2,1)`. `isSafe` = true.
7.  **Masuk (2,1)**. Tandai visited. Tambah ke jalur.
8.  Dari (2,1), coba arah **kanan (R)** ke `(2,2)`. `isSafe` = true.
9.  **Masuk (2,2)**. Tandai visited. Tambah ke jalur.
10. Dari (2,2), coba arah **kanan (R)** ke `(2,3)`. `isSafe` = true.
11. **Masuk (2,3)**. Tandai visited. Tambah ke jalur.
12. Dari (2,3), coba arah **bawah (D)** ke `(3,3)`. `isSafe` = true.
13. **Masuk (3,3)**. Tandai visited. Tambah ke jalur. **Goal tercapai!** Kembali `true`.

**Jalur DFS yang ditemukan:** `(0,0) → (0,1) → (1,1) → (2,1) → (2,2) → (2,3) → (3,3)`
**Representasi arah:** `RDRRDD`

---

## 💻 Contoh Kode (C++)

```cpp
#include <iostream>
#include <vector>
#include <cstring> // Untuk memset
#include <utility> // Untuk std::pair

using namespace std;

// Konstanta ukuran maze (N x N)
const int N = 4;

// Representasi maze
int maze[N][N] = {
    {1, 1, 0, 1},
    {0, 1, 0, 1},
    {1, 1, 1, 1},
    {0, 0, 1, 1}
};

// Matriks boolean untuk menandai sel yang sudah dikunjungi
bool visited[N][N]; 

// Vector untuk menyimpan jalur yang ditemukan
vector<pair<int, int>> path; 

// Array untuk arah pergerakan (kanan, bawah, kiri, atas)
// dx: perubahan pada koordinat x (baris)
// dy: perubahan pada koordinat y (kolom)
int dx[] = {0, 1, 0, -1}; // {kanan, bawah, kiri, atas} untuk x
int dy[] = {1, 0, -1, 0}; // {kanan, bawah, kiri, atas} untuk y

// Fungsi untuk mengecek apakah posisi (x, y) valid dan aman untuk dikunjungi
bool isSafe(int x, int y) {
    // Posisi harus dalam batas maze (0 hingga N-1)
    // Sel di posisi tersebut harus bernilai 1 (jalan yang bisa dilalui)
    // Sel di posisi tersebut belum dikunjungi dalam jalur saat ini
    return (x >= 0 && x < N && y >= 0 && y < N && 
            maze[x][y] == 1 && !visited[x][y]);
}

// Fungsi Depth-First Search rekursif
// x, y: koordinat posisi saat ini
bool dfs(int x, int y) {
    // Tandai sel saat ini sebagai sudah dikunjungi
    visited[x][y] = true;
    // Tambahkan sel saat ini ke jalur
    path.push_back({x, y});

    // Basis Kasus: Jika mencapai sel tujuan (pojok kanan bawah)
    if (x == N - 1 && y == N - 1) {
        return true; // Solusi ditemukan
    }

    // Jelajahi ke 4 arah yang mungkin (kanan, bawah, kiri, atas)
    for (int i = 0; i < 4; i++) {
        int nextX = x + dx[i]; // Hitung koordinat x berikutnya
        int nextY = y + dy[i]; // Hitung koordinat y berikutnya

        // Jika posisi berikutnya aman dan valid
        if (isSafe(nextX, nextY)) {
            // Lakukan panggilan rekursif DFS ke posisi berikutnya
            if (dfs(nextX, nextY)) {
                return true; // Jika panggilan rekursif menemukan solusi, kembalikan true
            }
        }
    }

    // Backtrack: Jika dari posisi (x,y) tidak ada jalur yang mengarah ke solusi
    // Hapus sel saat ini dari jalur (mundur)
    path.pop_back();
    return false; // Kembalikan false, menandakan jalur ini tidak berhasil
}

int main() {
    // Inisialisasi semua sel 'visited' menjadi false
    memset(visited, false, sizeof(visited));

    // Mulai DFS dari (0,0)
    if (dfs(0, 0)) {
        cout << "Jalur ditemukan:\n";
        // Cetak jalur yang ditemukan
        for (const auto& p : path) {
            cout << "(" << p.first << ", " << p.second << ") ";
        }
        cout << endl;
    } else {
        cout << "Tidak ada jalur yang tersedia untuk mencapai tujuan.\n";
    }

    return 0;
}
```

---

## 📚 Analisis Kompleksitas

### Kompleksitas Waktu

* **Setiap Simpul dan Sisi Dikunjungi Sekali**: Sama seperti BFS, DFS juga menjelajahi setiap simpul (`V`) dan setiap sisi (`E`) dalam graf paling banyak satu kali.
* **Graf dengan V Simpul dan E Sisi**: Kompleksitas waktu DFS adalah **$O(V + E)$**.
* **Untuk Grid (N x N)**: Jika diterapkan pada grid (seperti *maze*), waktu yang dibutuhkan sebanding dengan jumlah sel yang bisa dikunjungi. Dalam kasus terburuk (semua sel bisa dilalui), ini adalah **$O(N^2)$**.

### Kompleksitas Ruang

* **Stack Rekursi (atau Stack Eksplisit)**: Kedalaman maksimum *stack* rekursi (atau ukuran *stack* eksplisit) bisa mencapai $O(V)$ dalam kasus graf yang sangat dalam atau linier.
* **`Visited` Array**: Membutuhkan $O(V)$ ruang untuk melacak simpul yang sudah dikunjungi.
* **Path Tracking (opsional)**: Jika kita ingin menyimpan jalur, kita memerlukan array tambahan $O(V)$ untuk menyimpan simpul di jalur saat ini.

**Total kompleksitas ruang**: **$O(V)$** (atau $O(N^2)$ untuk grid $N \times N$).

---

## 🌟 Aplikasi Dunia Nyata

DFS adalah algoritma yang sangat fleksibel dengan banyak aplikasi:

* **Pencarian Jalur di *Game* dan Labirin**: Mirip dengan BFS, tetapi cenderung mencari satu jalur lebih cepat, meskipun bukan yang terpendek.
* **Penyusunan Urutan Topologis (Topological Sort)**: Mengatur tugas atau mata kuliah yang memiliki ketergantungan.
* **Deteksi Siklus dalam Graf**: Mengidentifikasi apakah ada lingkaran dalam struktur graf.
* **Pemecahan Teka-Teki**: Seperti Sudoku, puzzle 8-ubin, atau masalah pencarian solusi ruang keadaan lainnya.
* **Kompresi Data**: Dalam beberapa algoritma kompresi (misalnya, saat *traversal* pohon Huffman untuk membangun kode).
* **Connected Components**: Menemukan semua simpul yang saling terhubung dalam graf.

---

## 💪 Kekuatan dan Keterbatasan

### Kekuatan

* **Penggunaan Memori Lebih Efisien**: Untuk graf yang sangat lebar dan dangkal, DFS biasanya membutuhkan lebih sedikit memori daripada BFS karena kedalaman *stack* rekursinya lebih kecil daripada lebar antrian BFS.
* **Mudah Diimplementasikan**: Terutama dalam bentuk rekursif, DFS relatif sederhana untuk dikodekan.
* **Cocok untuk Menemukan Solusi Kedalaman Maksimal**: Jika solusi target cenderung berada jauh di dalam graf, DFS dapat menemukannya lebih cepat.
* **Digunakan dalam Banyak Algoritma Penting**: Merupakan fondasi untuk algoritma lain seperti *Topological Sort* dan deteksi siklus.

### Keterbatasan

* **Tidak Menjamin Solusi Terbaik (Terpendek)**: Untuk graf tak berbobot, DFS tidak akan selalu menemukan jalur terpendek. Ini akan menemukan jalur pertama yang ditemukannya.
* **Bisa Masuk ke *Infinite Loop***: Pada graf yang memiliki siklus (loop), DFS perlu mekanisme `visited` untuk menghindari kunjungan berulang tak terbatas.
* **Kurang Efisien di Graf Luas tapi Dangkal**: Jika solusi berada dekat dengan simpul awal tetapi di cabang yang tidak dieksplorasi DFS duluan, DFS bisa memakan waktu lama untuk menemukannya.
* **Risiko Stack Overflow**: Untuk graf yang sangat dalam, panggilan rekursif bisa menyebabkan *stack overflow* jika tidak dikelola dengan baik (misalnya, dengan menggunakan stack eksplisit).

---

## 🏁 Kesimpulan

**Depth-First Search (DFS)** adalah algoritma penelusuran graf atau *grid* yang esensial, bekerja dengan cara mengeksplorasi simpul **sedalam mungkin** sebelum kembali ke simpul sebelumnya (*backtrack*). DFS secara alami menerapkan konsep *backtracking*, menjadikannya sangat cocok untuk menyelesaikan masalah pencarian jalur dan kombinatorial. Efisien digunakan dalam banyak skenario, termasuk pemecahan labirin, deteksi siklus dalam graf, penyusunan topologis, hingga logika permainan dan kecerdasan buatan. Memahami DFS dan kekuatannya dalam menelusuri kedalaman adalah kunci dalam pengembangan algoritma yang efisien.

---