---
title: BREADTH-FIRST SEARCH
date: 2025-05-27 08:00:00 +0800
categories: [DESAIN ANALISIS ALGORITMA, BACKTRACKING ALGORITHM]
tags: [daa, algorithm, backtracking, bfs, c++]
---

![Desktop View](/assets/images/post/BFS.png)

# Menjelajahi Jaringan: Memahami Breadth-First Search (BFS)

## 📶 Pengantar: Mencari yang Terdekat Dahulu

Pernahkah Anda mencari rute tercepat di peta atau menjelajahi semua teman terdekat di media sosial Anda? Algoritma di balik itu kemungkinan besar adalah **Breadth-First Search (BFS)**!

**Breadth-First Search (BFS)** adalah metode fundamental untuk menjelajahi **graf** atau **pohon**. Intinya, BFS akan menelusuri simpul-simpul berdasarkan **jaraknya dari titik awal**. Secara sederhana, ia akan mengeksplorasi semua simpul yang paling dekat terlebih dahulu, baru kemudian beralih ke simpul yang lebih jauh. Pendekatannya menyebar ke seluruh arah secara merata dari titik pusat.

Bayangkan Anda mencari ruang kelas di sebuah gedung kampus berlantai 3. Anda pasti akan memeriksa seluruh ruangan di lantai pertama, kemudian semua ruangan di lantai kedua, sebelum akhirnya ke lantai atas. Itulah cara kerja BFS: menjelajah secara **mendatar** atau **per lapisan (level-by-level)**.

BFS banyak digunakan dalam berbagai aplikasi, mulai dari pencarian rute tercepat dalam *game*, penelusuran jaringan sosial, hingga pencarian jalur optimal dalam sistem navigasi dan peta digital.

---

## 🧠 Konsep Utama: Antrian dan Penelusuran Berlapis

BFS memiliki beberapa karakteristik kunci yang membedakannya:

1.  **Struktur Data Kunci**: BFS secara fundamental menggunakan struktur data **antrian (queue)**. Ini memastikan bahwa simpul yang pertama kali ditemukan juga yang pertama kali dieksplorasi secara menyeluruh.
2.  **Mekanisme Penjelajahan**:
    * Dimulai dari **simpul awal**.
    * Kunjungi **semua tetangga** langsung dari simpul awal tersebut.
    * Kemudian, lanjutkan ke **tetangga dari tetangga** (yaitu, simpul-simpul di "lapisan" berikutnya), dan seterusnya.
3.  **Sifat Traversal**: BFS adalah algoritma *level-order traversal* atau *breadth-wise traversal*. Ini berarti semua simpul pada kedalaman $k$ akan dikunjungi sebelum mengunjungi simpul pada kedalaman $k+1$.

---

## 📥 Input & 📤 Output

### Input

Untuk menjalankan BFS, kita memerlukan:

* **Representasi Graf**:
    * **Matriks Adjacency**: Cocok untuk graf kecil atau padat.
    * **Daftar Adjacency (Adjacency List)**: Lebih efisien untuk graf besar dan jarang (*sparse*).
    * **Matriks 2D**: Untuk kasus seperti pencarian jalur di *maze* atau *grid*.
* **Titik Awal (Source Node)**: Simpul dari mana penelusuran akan dimulai.
* **Titik Tujuan (opsional)**: Jika Anda ingin menemukan jalur dari *source* ke *target* tertentu.

### Output

Tergantung pada tujuan BFS, output bisa berupa:

* **Urutan Simpul yang Dikunjungi**: Daftar simpul yang dijelajahi selama proses pencarian, dalam urutan BFS.
* **Jalur Terpendek**: Jika diminta dan grafnya tidak berbobot, BFS akan menemukan jalur terpendek dari *source* ke *target*.
* **Jarak Minimum**: Jarak (jumlah *edge*) minimum dari *source* ke semua simpul lain yang bisa dijangkau (jika graf tidak berbobot).

---

## 🧮 Langkah-langkah Penyelesaian BFS

Mari kita uraikan proses BFS:

1.  **Inisialisasi**:
    * Tentukan **simpul awal (start node)**.
    * Buat struktur data **`queue`** dan masukkan simpul awal ke dalamnya.
    * Buat struktur data **`visited`** (biasanya array boolean atau set) untuk melacak simpul yang sudah dikunjungi, dan tandai simpul awal sebagai dikunjungi.

2.  **Proses Iteratif**:
    * Selama **antrian tidak kosong**:
        * **Keluarkan (dequeue)** simpul paling depan dari antrian. Sebut saja `current_node`.
        * (Opsional) Lakukan proses pada `current_node` (misalnya, cetak, cek apakah ini tujuan, dll.).
        * **Telusuri Tetangga**: Untuk **setiap tetangga** dari `current_node`:
            * Jika tetangga tersebut **belum dikunjungi**:
                * Tandai tetangga sebagai **dikunjungi**.
                * **Tambahkan (enqueue)** tetangga tersebut ke antrian.
                * (Jika mencari jalur, catat `current_node` sebagai `parent` dari tetangga ini).

3.  **Selesai**: Proses berlanjut hingga antrian kosong (semua simpul yang terhubung telah dijelajahi) atau simpul tujuan ditemukan.

---

## 🧩 Problem and Solution Example

**Misal kita memiliki Maze Grid (4x4):**

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

**Langkah Solusi (dengan mencatat jalur):**

1.  Inisialisasi: `Queue: [(0,0)]`, `Visited: {(0,0)}`, `Parent: {}`
2.  **Keluarkan (0,0)**. Tetangga valid yang belum dikunjungi: `(0,1)`.
    * `Queue: [(0,1)]`, `Visited: {(0,0), (0,1)}`, `Parent: {(0,1): (0,0)}`
3.  **Keluarkan (0,1)**. Tetangga valid yang belum dikunjungi: `(1,1)`.
    * `Queue: [(1,1)]`, `Visited: {(0,0), (0,1), (1,1)}`, `Parent: {(0,1): (0,0), (1,1): (0,1)}`
4.  **Keluarkan (1,1)**. Tetangga valid yang belum dikunjungi: `(2,1)`.
    * `Queue: [(2,1)]`, `Visited: {...}`, `Parent: {..., (2,1): (1,1)}`
5.  **Keluarkan (2,1)**. Tetangga valid yang belum dikunjungi: `(2,2)`.
    * `Queue: [(2,2)]`, `Visited: {...}`, `Parent: {..., (2,2): (2,1)}`
6.  **Keluarkan (2,2)**. Tetangga valid yang belum dikunjungi: `(2,3)`.
    * `Queue: [(2,3)]`, `Visited: {...}`, `Parent: {..., (2,3): (2,2)}`
7.  **Keluarkan (2,3)**. Tetangga valid yang belum dikunjungi: `(3,3)`.
    * `Queue: [(3,3)]`, `Visited: {...}`, `Parent: {..., (3,3): (2,3)}`
    * *(3,3) adalah Goal! Berhenti.*

**Jalur terpendek (rekonstruksi dari Parent):**
`(0,0) → (0,1) → (1,1) → (2,1) → (2,2) → (2,3) → (3,3)`

**Representasi Arah (misal: R=Kanan, D=Bawah, L=Kiri, U=Atas):** `RDRRDD`

---

## 💻 Contoh Kode (C++)

```cpp
#include <iostream>
#include <vector>
#include <queue> // Untuk std::queue

using namespace std;

// Fungsi Breadth-First Search (BFS)
// start: simpul awal untuk pencarian
// adjList: representasi graf menggunakan adjacency list
// V: jumlah total simpul dalam graf
void bfs(int start, const vector<vector<int>>& adjList, int V) {
    // vector boolean untuk melacak simpul yang sudah dikunjungi
    // Inisialisasi semua simpul sebagai belum dikunjungi (false)
    vector<bool> visited(V, false); 
    
    // Antrian (queue) untuk menyimpan simpul yang akan dikunjungi
    queue<int> q;

    // Langkah 1: Mulai dari simpul awal
    visited[start] = true; // Tandai simpul awal sebagai sudah dikunjungi
    q.push(start);         // Masukkan simpul awal ke dalam antrian

    cout << "Urutan kunjungan BFS dari node " << start << ": ";

    // Langkah 2: Proses iteratif selama antrian tidak kosong
    while (!q.empty()) {
        int node = q.front(); // Ambil simpul paling depan dari antrian
        q.pop();               // Hapus simpul tersebut dari antrian
        cout << node << " ";   // Cetak simpul yang sedang dikunjungi

        // Langkah 3: Telusuri semua tetangga dari simpul saat ini
        // adjList[node] memberikan daftar semua tetangga dari 'node'
        for (int neighbor : adjList[node]) {
            // Jika tetangga belum dikunjungi
            if (!visited[neighbor]) {
                visited[neighbor] = true; // Tandai sebagai sudah dikunjungi
                q.push(neighbor);         // Masukkan tetangga ke dalam antrian
            }
        }
    }

    cout << endl; // Baris baru setelah semua simpul dicetak
}

int main() {
    int V = 6; // Definisikan jumlah simpul (nodes) dalam graf

    // Buat adjacency list untuk merepresentasikan graf
    // adjList[i] akan berisi vector dari simpul-simpul yang bertetangga dengan simpul i
    vector<vector<int>> adjList(V);

    // Tambahkan sisi-sisi (edges) ke graf
    // Ini adalah contoh graf tidak berbobot dan tidak berarah
    // Sisi ditambahkan dua arah karena tidak berarah (misal, 0 terhubung ke 1, maka 1 juga terhubung ke 0)
    adjList[0] = {1, 2};
    adjList[1] = {0, 3, 4};
    adjList[2] = {0, 4};
    adjList[3] = {1, 5};
    adjList[4] = {1, 2, 5};
    adjList[5] = {3, 4};

    int startNode = 0; // Tentukan simpul awal untuk memulai BFS

    // Panggil fungsi BFS
    bfs(startNode, adjList, V);

    return 0; // Program berakhir dengan sukses
}
```

---

## 📚 Analisis Kompleksitas

### Kompleksitas Waktu

* **Setiap Simpul dan Sisi Dikunjungi Sekali**: BFS mengunjungi setiap simpul (`V`) dan setiap sisi (`E`) dalam graf paling banyak satu kali.
* **Graf dengan V Simpul dan E Sisi**: Kompleksitas waktu BFS adalah **$O(V + E)$**.
    * Jika graf direpresentasikan dengan matriks adjacency, mungkin menjadi $O(V^2)$ karena iterasi melalui baris/kolom matriks. Namun, dengan adjacency list, itu $O(V+E)$.
* **Untuk Grid (N x N)**: Jika diterapkan pada grid (seperti *maze*), waktu yang dibutuhkan sebanding dengan jumlah sel yang bisa dikunjungi. Dalam kasus terburuk (semua sel bisa dilalui), ini adalah $O(N^2)$.

### Kompleksitas Ruang

* **Struktur Data yang Digunakan**:
    * **`Queue`**: Dalam kasus terburuk, antrian bisa menyimpan semua simpul pada satu "lapisan" yang sama. Jumlah maksimum simpul yang bisa ada di antrian adalah $O(V)$.
    * **`Visited` Array**: Membutuhkan $O(V)$ ruang untuk melacak simpul yang sudah dikunjungi.
    * **Parent/Path Tracking (opsional)**: Jika kita ingin merekonstruksi jalur, kita memerlukan array tambahan $O(V)$ untuk menyimpan simpul "parent".
* **Untuk Grid (N x N)**: Kompleksitas ruang adalah $O(N^2)$ untuk menyimpan status `visited` dan antrian dalam kasus terburuk.

---

## 🌟 Aplikasi Dunia Nyata

BFS adalah algoritma yang sangat serbaguna dan digunakan di berbagai bidang:

* **Pencarian Jalur Terpendek (Unweighted Graphs)**: Karena BFS menjelajahi lapis demi lapis, ia secara alami menemukan jalur terpendek (dalam jumlah *edge*) di graf yang tidak memiliki bobot pada sisinya. Ini sempurna untuk:
    * **Navigasi/Peta Digital**: Menemukan rute terpendek di Google Maps, Grab, Gojek (jika tidak mempertimbangkan bobot jalan).
    * **Pencarian Labirin**: Menemukan jalan keluar dari labirin.
* **Jaringan Sosial**: Menemukan "derajat perpisahan" (misalnya, berapa banyak teman antara dua orang), atau menjelajahi teman-teman di tingkat tertentu.
* **Jaringan Komputer**: Melakukan *network crawling*, mencari *node* terdekat, atau membanjiri paket data (jika *broadcast*).
* **Web Crawlers**: Mesin pencari menggunakan BFS untuk menjelajahi halaman web dari satu tautan ke tautan lainnya.
* **Pencarian Status Game**: Dalam *game* atau AI, mencari solusi dari satu status ke status lain (misalnya, menyelesaikan Rubik's Cube dalam jumlah gerakan minimum).

---

## 💪 Kekuatan dan Keterbatasan

### Kekuatan

* **Kelengkapan (Completeness)**: BFS dijamin akan menemukan solusi jika ada.
* **Optimalitas (Optimality)**: Untuk graf yang tidak berbobot (*unweighted graphs*), BFS dijamin akan menemukan **jalur terpendek**. Ini adalah keuntungan besar dibandingkan DFS.
* **Sederhana untuk Dipahami dan Diimplementasikan**: Konsepnya intuitif dan mudah dikodekan.

### Keterbatasan

* **Memori Intensif**: Untuk graf yang sangat lebar (banyak tetangga di setiap lapisan), antrian bisa tumbuh sangat besar, membutuhkan banyak memori.
* **Waktu untuk Graf Lebar**: Meskipun $O(V+E)$, jika graf sangat lebar, jumlah $E$ bisa sangat besar, menyebabkan waktu eksekusi yang lama.
* **Tidak Optimal untuk Graf Berbobot**: Jika sisi memiliki bobot (misalnya, jarak antar kota), BFS tidak akan menjamin jalur terpendek. Algoritma seperti Dijkstra atau A* lebih cocok.

---

## 🏁 Kesimpulan

**Breadth-First Search (BFS)** adalah algoritma penelusuran graf atau pohon yang kuat dan fundamental. Dengan menggunakan struktur data **antrian**, ia menjelajahi simpul secara **berlapis** (dari yang terdekat hingga terjauh), memastikan semua simpul dengan jarak terpendek dijelajahi terlebih dahulu. Hal ini menjadikan BFS sangat cocok untuk mencari **jalur terpendek pada graf tak berbobot** dan efektif digunakan dalam berbagai aplikasi dunia nyata, mulai dari *game*, peta, jaringan sosial, hingga pencarian data. Memahami BFS adalah langkah penting dalam menguasai algoritma graf.

---