---
title: KAHN'S ALGORITHM
date: 2025-05-27 08:00:00 +0800
categories: [DESAIN ANALISIS ALGORITMA, KAHN'S ALGORITHM]
tags: [daa, algorithm, backtracking, kahn, c++]     # TAG names should always be lowercase
---

![Desktop View](/assets/images/post/KAHN.png)

# Mengurutkan Ketergantungan: Memahami Kahn's Algorithm (Topological Sort)

## 🗺️ Pengantar: Mengurai Urutan Tugas

Pernahkah Anda menyusun daftar tugas yang saling bergantung, seperti prasyarat mata kuliah atau langkah-langkah kompilasi kode? Di sinilah **Topological Sorting** dan **Kahn's Algorithm** menjadi sangat berguna!

**Kahn’s Algorithm** adalah sebuah metode **penyusunan topologi (topological sorting)** yang digunakan untuk menentukan urutan linier simpul-simpul dalam sebuah **Directed Acyclic Graph (DAG)** berdasarkan ketergantungan antar simpul.

Secara sederhana:
* Ini adalah pendekatan **berbasis BFS (Breadth-First Search)** untuk menemukan urutan simpul yang valid.
* Urutan topologi DAG adalah urutan di mana, untuk setiap sisi berarah $u \rightarrow v$, simpul $u$ selalu muncul **sebelum** simpul $v$ dalam urutan.
* Jika graf memiliki siklus (bukan DAG), maka topological sort tidak mungkin dilakukan.

---

## 🧠 Konsep Utama: `In-Degree` dan Antrean

Kahn's Algorithm bekerja berdasarkan prinsip-prinsip berikut:

1.  **In-Degree**: Konsep kunci adalah **`in-degree`** (derajat masuk) setiap simpul, yaitu jumlah sisi yang mengarah ke simpul tersebut.
2.  **Simpul Awal**: Semua simpul yang tidak memiliki prasyarat (yaitu, memiliki `in-degree` = 0) adalah simpul yang bisa dimulai terlebih dahulu. Simpul-simpul ini akan menjadi titik awal penelusuran dan dimasukkan ke dalam **antrean (queue)**.
3.  **Proses Iteratif**: Algoritma memproses simpul dari antrean satu per satu. Setiap kali sebuah simpul diproses, ia "menghilangkan" dirinya sebagai prasyarat bagi simpul-simpul yang bergantung padanya, sehingga `in-degree` tetangganya berkurang.
4.  **Deteksi Siklus**: Jika pada akhirnya tidak semua simpul dapat dimasukkan ke dalam urutan topologi (yaitu, antrean menjadi kosong sebelum semua simpul diproses), berarti graf tersebut memiliki siklus.

---

## 🧑‍💻 Konsep Backtracking dalam Kahn's Algorithm

Meskipun **Kahn’s Algorithm** tidak secara eksplisit menggunakan mekanisme *backtracking* yang sama seperti pada DFS (yang rekursif dan "mundur" saat jalan buntu), atau algoritma eksploratif lain seperti N-Queens, ia tetap menyelesaikan masalah dependensi secara inkremental dan selektif.

Dalam konteks *topological sorting*, pemilihan simpul dengan `in-degree = 0` dapat dianggap sebagai **“langkah maju”**. Algoritma ini bersifat *greedy* dan berbasis antrean (*BFS-style*), sehingga ia tidak pernah "mundur" dari keputusan yang telah diambil.

**Namun, ada analogi dengan *backtracking*:**

* Jika pada akhirnya `queue` menjadi kosong dan kita belum memproses semua simpul dalam graf, ini berarti ada **siklus** dalam graf. Kondisi ini dapat dianggap sebagai "jalan buntu" atau kegagalan yang menyerupai konsep *backtracking* — yaitu kondisi tidak ada solusi valid yang bisa dicapai dan perlu "kembali" meninjau struktur graf (untuk mendeteksi siklus).
* **Penting**: Kode C++ yang Anda berikan sebenarnya mencari *semua* kemungkinan urutan topologis (jika ada lebih dari satu), yang memang **menggunakan *backtracking* rekursif** untuk menjelajahi setiap cabang pilihan. Ini adalah variasi dari Kahn's Algorithm yang dimodifikasi untuk menghasilkan semua kemungkinan urutan, bukan hanya satu seperti implementasi Kahn standar.

---

## 📥 Input & 📤 Output

### Input

* Sebuah **graf berarah (`Directed Graph`)** dalam bentuk:
    * **Matriks Adjacency**: Jika graf kecil.
    * **Daftar Adjacency (Adjacency List)**: Jika graf besar atau *sparse*.
* Jumlah simpul (`V`) dan jumlah sisi (`E`).
* Setiap sisi merepresentasikan **ketergantungan** (misalnya: $A \rightarrow B$ berarti $A$ harus dilakukan sebelum $B$).

### Output

* **Urutan Simpul Secara Topologis**: Satu atau semua urutan valid berdasarkan dependensi (tergantung implementasi).
* **Deteksi Siklus**: Jika ada siklus, output menyatakan "Graf mengandung siklus, tidak bisa dilakukan topological sort."

---

## 🧮 Langkah-langkah Penyelesaian (Kahn's Algorithm Standar)

1.  **Hitung `in-degree`**: Untuk setiap simpul dalam graf, hitung `in-degree` (jumlah sisi yang masuk ke simpul tersebut).
2.  **Inisialisasi Antrean**: Buat sebuah antrean (`queue`). Masukkan semua simpul yang memiliki `in-degree` sama dengan 0 ke dalam antrean.
3.  **Proses Antrean**:
    * Selama antrean tidak kosong:
        * Ambil simpul `u` dari depan antrean dan **tambahkan `u` ke daftar hasil urutan topologis**.
        * Untuk **setiap tetangga `v`** dari simpul `u` (yaitu, ada sisi $u \rightarrow v$):
            * **Kurangi `in-degree` dari `v`** sebesar 1.
            * Jika `in-degree` dari `v` menjadi 0, **tambahkan `v` ke dalam antrean**.
4.  **Verifikasi & Deteksi Siklus**:
    * Setelah antrean kosong, hitung jumlah simpul yang berhasil ditambahkan ke urutan topologis.
    * Jika jumlahnya sama dengan total jumlah simpul di graf (`V`), maka *topological sort* berhasil, dan urutan yang dihasilkan adalah valid.
    * Jika jumlahnya **kurang dari `V`**, berarti ada siklus dalam graf, dan *topological sort* tidak mungkin dilakukan.

---

## 🧩 Problem and Solution Example

**Misalnya kita punya dependensi antar mata kuliah:**

* $A \rightarrow C$ (A adalah prasyarat C)
* $B \rightarrow C$ (B adalah prasyarat C)
* $C \rightarrow D$ (C adalah prasyarat D)

**Representasi Graf:**

```
  A ----> C ----> D
  ^       ^
  |       |
  B ------+
```

**Langkah Solusi (Kahn's Algorithm Standar):**

1.  **In-degree awal:**
    * A: 0
    * B: 0
    * C: 2 (dari A dan B)
    * D: 1 (dari C)

2.  **Inisialisasi Antrean:**
    * `Queue = [A, B]` (karena `in-degree` A dan B adalah 0)
    * `Topological Order = []`
    * `Count Processed Nodes = 0`

3.  **Proses Iteratif:**

    * **Ambil A dari Queue**:
        * `Topological Order = [A]`
        * `Count Processed Nodes = 1`
        * Tetangga A adalah C. Kurangi `in-degree[C]` menjadi 1. (`in-degree[C]` sekarang: 1)
        * `Queue = [B]`

    * **Ambil B dari Queue**:
        * `Topological Order = [A, B]`
        * `Count Processed Nodes = 2`
        * Tetangga B adalah C. Kurangi `in-degree[C]` menjadi 0. (`in-degree[C]` sekarang: 0)
        * Karena `in-degree[C]` menjadi 0, masukkan C ke Queue.
        * `Queue = [C]`

    * **Ambil C dari Queue**:
        * `Topological Order = [A, B, C]`
        * `Count Processed Nodes = 3`
        * Tetangga C adalah D. Kurangi `in-degree[D]` menjadi 0. (`in-degree[D]` sekarang: 0)
        * Karena `in-degree[D]` menjadi 0, masukkan D ke Queue.
        * `Queue = [D]`

    * **Ambil D dari Queue**:
        * `Topological Order = [A, B, C, D]`
        * `Count Processed Nodes = 4`
        * D tidak punya tetangga.
        * `Queue = []`

4.  **Verifikasi**:
    * `Count Processed Nodes` (4) sama dengan total simpul (`V=4`).
    * **Topological sort berhasil!**

**Urutan Topologis Valid**: `A, B, C, D` (atau `B, A, C, D`).

---

## 💻 Contoh Kode (C++ - Mencari Semua Topological Sorts)

```cpp
#include <iostream>
#include <vector>
#include <unordered_map> // Untuk adjacency list dan in-degree
#include <set>           // Untuk menyimpan semua simpul unik
#include <algorithm>     // Untuk algoritma standar (tidak digunakan secara langsung di sini, tapi umumnya)

using namespace std;

// Representasi graf menggunakan adjacency list
unordered_map<char, vector<char>> graph;
// Menyimpan in-degree untuk setiap simpul
unordered_map<char, int> in_degree;
// Set untuk menyimpan semua simpul yang ada di graf
set<char> vertices;

// Fungsi rekursif untuk menemukan semua urutan topologis yang mungkin
// res: vector yang menyimpan urutan topologis saat ini yang sedang dibangun
// visited: array boolean untuk melacak apakah simpul sudah dipilih dalam urutan saat ini
void allTopologicalSorts(vector<char> &res, vector<bool> &visited) {
    bool found_node_to_process = false; // Flag untuk mengetahui apakah ada simpul yang bisa diproses
    
    // Iterasi melalui semua simpul yang ada di graf
    for (char v : vertices) {
        // Kondisi untuk memilih simpul 'v':
        // 1. Belum dikunjungi (belum ada di 'res' saat ini)
        // 2. In-degree-nya adalah 0 (tidak ada prasyarat yang belum terpenuhi)
        if (!visited[v] && in_degree[v] == 0) {
            found_node_to_process = true; // Set flag karena kita menemukan simpul yang bisa diproses

            // --- Pilih simpul v (langkah "maju") ---
            visited[v] = true;     // Tandai v sebagai sudah dipilih
            res.push_back(v);      // Tambahkan v ke urutan hasil

            // Kurangi in-degree dari semua tetangga v
            // Ini mensimulasikan v telah selesai diproses
            for (char neighbor : graph[v]) {
                in_degree[neighbor]--;
            }

            // Panggil rekursi untuk melanjutkan membangun urutan
            allTopologicalSorts(res, visited);

            // --- Backtrack (kembali ke kondisi sebelum memilih v) ---
            // Batalkan penandaan v sebagai sudah dipilih
            visited[v] = false;
            // Hapus v dari urutan hasil (kembali ke state sebelumnya)
            res.pop_back();
            // Kembalikan in-degree tetangga ke nilai semula (batalkan efek pemilihan v)
            for (char neighbor : graph[v]) {
                in_degree[neighbor]++;
            }
        }
    }

    // Basis Kasus: Jika tidak ada simpul yang bisa diproses (`!found_node_to_process`)
    // Ini berarti:
    // 1. Kita sudah berhasil membangun urutan topologis yang lengkap (res.size() == vertices.size())
    // ATAU
    // 2. Ada siklus (in-degree tidak ada yang 0, tapi belum semua simpul diproses)
    // Dalam konteks fungsi ini yang mencari *semua* sort, ini berarti kita telah mencapai akhir dari satu jalur sort.
    if (!found_node_to_process) {
        // Cetak urutan topologis yang ditemukan
        for (char c : res) {
            cout << c << " ";
        }
        cout << endl;
    }
}

int main() {
    int E; // Jumlah sisi (edges)
    cout << "Masukkan jumlah edge (sisi): ";
    cin >> E;

    cout << "Masukkan sisi dalam format 'u v' (u -> v):" << endl;
    for (int i = 0; i < E; ++i) {
        char u, v;
        cin >> u >> v;
        // Tambahkan sisi ke graf
        graph[u].push_back(v);
        // Tingkatkan in-degree simpul tujuan v
        in_degree[v]++;
        // Masukkan kedua simpul ke set vertices
        vertices.insert(u);
        vertices.insert(v);
    }

    // Pastikan semua simpul yang hanya menjadi sumber memiliki in-degree 0
    // (unordered_map tidak membuat entri jika key tidak ada, jadi perlu inisialisasi manual)
    for (char v : vertices) {
        if (in_degree.find(v) == in_degree.end()) {
            in_degree[v] = 0;
        }
    }

    vector<char> result_sequence; // Vector untuk menyimpan urutan topologis
    // Inisialisasi vector visited dengan ukuran yang cukup (misal 256 untuk char ASCII)
    vector<bool> visited(256, false); 

    cout << "Semua kemungkinan urutan topologis:" << endl;
    allTopologicalSorts(result_sequence, visited);

    // Tambahan: Mengecek apakah ada siklus (jika tidak ada output sama sekali)
    // Fungsi allTopologicalSorts akan mencetak semua urutan. Jika tidak ada yang dicetak
    // dan jumlah vertex > 0, berarti ada siklus.
    // Namun, cara yang lebih pasti untuk deteksi siklus di Kahn's Algorithm standar adalah 
    // membandingkan `count_processed_nodes` dengan `V`.
    // Implementasi di atas fokus mencari *semua* sort.

    return 0;
}
```

---

## 📚 Analisis Kompleksitas

### Kompleksitas Waktu

* **Kahn's Algorithm (Standar)**: $O(V + E)$
    * Menghitung `in-degree` semua simpul: $O(V + E)$.
    * Memproses semua simpul dan sisi satu kali: Setiap simpul dimasukkan dan diambil dari `queue` sekali ($O(V)$). Setiap sisi diperiksa sekali (saat mengurangi `in-degree` tetangga) ($O(E)$).
* **Contoh Kode di Atas (Mencari Semua Urutan Topologis)**: Ini jauh lebih kompleks. Karena ia menemukan *semua* kemungkinan urutan topologis (yang bisa ada banyak), kompleksitasnya menjadi **faktorial** terhadap jumlah simpul atau lebih buruk, yaitu $O(V! \times (V+E))$ atau $O(V! \times V^2)$ dalam kasus terburuk (jika banyak urutan dan setiap pengecekan memakan waktu). Ini adalah masalah NP-Hard jika diminta untuk menghitung semua urutan.

### Kompleksitas Ruang

* **Kahn's Algorithm (Standar)**: $O(V + E)$
    * Menyimpan graf (`adjacency list`): $O(V + E)$.
    * Menyimpan `in-degree` array/map: $O(V)$.
    * Menyimpan `queue` dan hasil urutan: $O(V)$.
* **Contoh Kode di Atas (Mencari Semua Urutan Topologis)**: $O(V + E + \text{jumlah_solusi} \times V)$
    * Tambahan ruang untuk `visited` array dan rekursi stack: $O(V)$.
    * Ruang untuk menyimpan `result_sequence`: $O(V)$ untuk setiap jalur. Jika semua solusi disimpan, bisa jadi sangat besar.

---

## 🌟 Aplikasi Dunia Nyata

Kahn's Algorithm (dan topological sorting secara umum) sangat penting untuk:

* **Penjadwalan Tugas**: Mengatur urutan tugas-tugas yang memiliki prasyarat dalam sebuah proyek atau proses manufaktur (misalnya, membuat jadwal pembangunan rumah).
* **Kompilasi Program**: Menentukan urutan kompilasi file sumber yang saling bergantung dalam sebuah proyek (`makefiles` menggunakan konsep ini).
* **Prasyarat Mata Kuliah**: Menyusun daftar mata kuliah yang harus diambil dalam urutan yang benar di universitas.
* **Evaluasi Sel di Spreadsheet**: Menentukan urutan evaluasi formula sel yang saling bergantung.
* **Serialisasi Data**: Menentukan urutan untuk menyimpan objek-objek yang saling mereferensi agar bisa direkonstruksi dengan benar.

---

## 💪 Kekuatan dan Keterbatasan

### Kekuatan

* **Kompleksitas Waktu Optimal (untuk algoritma standar)**: $O(V + E)$ adalah seefisien mungkin karena setiap simpul dan sisi harus dilihat setidaknya sekali.
* **Deteksi Siklus**: Algoritma ini secara inheren dapat mendeteksi apakah graf mengandung siklus (jika tidak semua simpul dapat diurutkan). Ini adalah keuntungan besar.
* **Sangat Cocok untuk Masalah Dependensi**: Merupakan solusi standar untuk mengurai dan mengurutkan tugas-tugas yang memiliki prasyarat.

### Keterbatasan

* **Hanya untuk DAG**: Tidak bisa digunakan pada graf yang mengandung siklus.
* **Output Tidak Unik (Biasanya)**: Untuk banyak DAG, ada lebih dari satu urutan topologis yang valid. Kahn's Algorithm standar hanya memberikan satu dari sekian banyak kemungkinan solusi, tanpa informasi tentang yang lain. (Kode contoh Anda mencoba menemukan *semua* solusi, yang merupakan kasus penggunaan yang berbeda dan lebih kompleks).
* **Memerlukan Struktur Data Tambahan**: Membutuhkan array `in-degree` dan `queue` tambahan.

---

## 🏁 Kesimpulan

**Kahn’s Algorithm** adalah metode yang efisien dan sistematis untuk melakukan **topological sorting** pada **Directed Acyclic Graph (DAG)**. Dengan memanfaatkan struktur `in-degree` dan antrean (`queue`), algoritma ini mampu menyusun simpul-simpul berdasarkan ketergantungan yang ada, serta mendeteksi keberadaan siklus dalam graf.

Implementasinya dalam bahasa C++ menunjukkan bagaimana struktur data seperti `unordered_map`, `queue` (meskipun kode contoh menggunakan rekursi dan *backtracking* untuk mencari *semua* solusi), dan `vector` dapat digunakan untuk memodelkan graf dan proses penyusunannya secara elegan. Dengan kompleksitas waktu $O(V + E)$ untuk versi standar, algoritma ini sangat berguna dalam berbagai aplikasi nyata, seperti penjadwalan tugas, kompilasi program, dan pengelolaan prasyarat mata kuliah.

---