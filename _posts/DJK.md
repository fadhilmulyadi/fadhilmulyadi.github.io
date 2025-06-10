---
title: DIJKSTRA'S ALGORITHM
date: 2025-05-27 08:00:00 +0800
categories: [DESAIN ANALISIS ALGORITMA, DIJKSTRA's ALGORITHM]
tags: [daa, algorithm, backtracking, dijkstra, c++]
---

![Desktop View](/assets/lib/images/post/DJK.png)

# Menemukan Jalur Terpendek: Memahami Algoritma Dijkstra

## 🗺️ Pengantar: Menjelajahi Jaringan dengan Cerdas

Bayangkan Anda ingin pergi dari satu tempat ke tempat lain di kota, dan Anda ingin menemukan rute tercepat atau terpendek. Bagaimana caranya? Di sinilah **Algoritma Dijkstra** berperan!

**Dijkstra's Algorithm** adalah sebuah metode yang digunakan untuk mencari **jalur terpendek** dari satu titik (`simpul/node`) awal ke semua titik (`simpul/node`) lain dalam sebuah **graf berbobot positif**. Graf ini bisa berupa jaringan yang terdiri dari simpul (titik) dan sisi (hubungan antar titik) dengan **bobot tertentu pada sisi-sisinya**. Bobot ini bisa merepresentasikan jarak, waktu, biaya, atau hambatan. Algoritma ini secara sistematis membantu kita menentukan jalur terpendek tersebut.

---

## 🧠 Konsep Utama: Greedy dan Prioritas

Algoritma Dijkstra bekerja berdasarkan prinsip *greedy* dengan eksplorasi bertahap:

1.  **Inisialisasi Jarak**: Setiap simpul (kecuali simpul awal) diasumsikan memiliki jarak tak hingga ($\infty$) dari simpul awal. Simpul awal memiliki jarak 0.
2.  **Pilih Simpul Terdekat**: Pada setiap langkah, algoritma akan selalu memilih simpul yang **belum dikunjungi** dan memiliki **jarak terpendek** dari simpul awal yang diketahui sejauh ini.
3.  **Perbarui Jarak Tetangga**: Dari simpul yang baru dipilih, algoritma akan memeriksa semua tetangganya. Jika ada jalur yang lebih pendek ke tetangga tersebut melalui simpul yang baru dipilih, maka jarak ke tetangga itu akan diperbarui.
4.  **Iterasi**: Langkah ini diulang hingga semua simpul yang dapat dijangkau dari simpul awal telah dikunjungi.

---

## 🧑‍💻 Konsep "Backtracking" dalam Dijkstra's Algorithm

**Dijkstra’s Algorithm** sendiri adalah algoritma *greedy* dan **bukan algoritma *backtracking* eksplisit** seperti DFS atau solusi N-Queens. Dijkstra membuat keputusan lokal terbaik di setiap langkah (memilih simpul terdekat yang belum dikunjungi) dan tidak pernah "mundur" dari keputusan tersebut untuk mencari jalur lain.

Namun, konsep pelacakan jalur terpendek setelah algoritma selesai dapat melibatkan *backtracking* secara implisit:

* Setelah Dijkstra menghitung jarak minimum dari simpul sumber ke semua simpul lain, ia juga dapat menyimpan `parent` (atau `predecessor`) untuk setiap simpul. `parent[v]` akan menyimpan simpul `u` yang merupakan simpul terakhir sebelum mencapai `v` melalui jalur terpendek.
* Untuk merekonstruksi jalur terpendek dari simpul sumber ke simpul tujuan tertentu, kita akan **melakukan *backtrack*** dari simpul tujuan kembali ke simpul sumber, mengikuti `parent` dari setiap simpul. Inilah bentuk *backtracking* yang terjadi dalam konteks Dijkstra: untuk **merekonstruksi jalur** setelah semua perhitungan jarak minimum selesai.

---

## 📥 Input & 📤 Output

### Input

* **Graf Berbobot Positif**: Ini adalah syarat utama Dijkstra. Bobot sisi harus non-negatif.
    * **Representasi**: Umumnya menggunakan **daftar adjacency (adjacency list)**, di mana setiap entri berisi pasangan `(node, weight)` atau `(simpul_tujuan, bobot_sisi)`.
* **Jumlah Simpul (V)** dan **Sisi (E)** dalam graf.
* **Simpul Awal (Source Node)**: Titik dari mana semua jalur terpendek akan dihitung.

### Output

* **Jarak Minimum**: Jarak terpendek dari simpul awal ke **semua** simpul lainnya dalam graf.
* **Jalur Terpendek (opsional)**: Jalur spesifik dari simpul awal ke simpul tujuan tertentu (direkonstruksi menggunakan informasi `parent`).

---

## 🧮 Langkah-langkah Penyelesaian

1.  **Inisialisasi**:
    * Buat array `dist` berukuran `V`, inisialisasi semua elemennya dengan $\infty$ (tak hingga).
    * Set `dist[source] = 0`.
    * Buat array `parent` berukuran `V`, inisialisasi semua elemennya dengan `-1` (atau null), untuk merekonstruksi jalur.
    * Gunakan **priority queue (min-heap)** untuk menyimpan pasangan `(jarak, simpul)`. Masukkan `(0, source)` ke dalam `priority queue`.

2.  **Proses Iteratif**:
    * Selama `priority queue` tidak kosong:
        * Ambil `(d, u)` dari puncak `priority queue`, di mana `u` adalah simpul dan `d` adalah jarak terpendek yang diketahui ke `u` saat ini.
        * Jika `d` lebih besar dari `dist[u]` (berarti kita sudah menemukan jalur yang lebih pendek ke `u` sebelumnya), lewati iterasi ini.
        * Untuk setiap tetangga `v` dari `u` dan bobot sisi `w` dari `u` ke `v`:
            * Jika `dist[u] + w < dist[v]` (yaitu, jalur melalui `u` lebih pendek dari jalur yang diketahui sebelumnya ke `v`):
                * Perbarui `dist[v] = dist[u] + w`.
                * Set `parent[v] = u` (simpan `u` sebagai pendahulu `v` di jalur terpendek).
                * Masukkan `(dist[v], v)` ke dalam `priority queue`.

3.  **Rekonstruksi Jalur (Opsional)**:
    * Setelah `priority queue` kosong (algoritma selesai), array `dist` akan berisi jarak terpendek dari `source` ke semua simpul.
    * Untuk merekonstruksi jalur ke simpul `target`, mulai dari `target` dan ikuti `parent` array sampai mencapai `source` (atau `-1`). Balik urutannya untuk mendapatkan jalur dari `source` ke `target`.

---

## 🧩 Problem and Solution Example

**Graf Berbobot:**

```
A --1--> B
A --4--> C
B --2--> C
B --6--> D
C --3--> D
```

**Source**: `A`

**Langkah Solusi:**

1.  **Inisialisasi**:
    * `dist = {A:0, B:∞, C:∞, D:∞}`
    * `parent = {A:-1, B:-1, C:-1, D:-1}`
    * `PQ = [(0, A)]` (jarak, simpul)

2.  **Proses Simpul A (0)**:
    * Ambil `(0, A)` dari PQ.
    * Tetangga B: `dist[A] + 1 = 0 + 1 = 1`. Karena `1 < ∞`, `dist[B] = 1`, `parent[B] = A`. Masukkan `(1, B)` ke PQ.
    * Tetangga C: `dist[A] + 4 = 0 + 4 = 4`. Karena `4 < ∞`, `dist[C] = 4`, `parent[C] = A`. Masukkan `(4, C)` ke PQ.
    * `PQ = [(1, B), (4, C)]` (diurutkan berdasarkan jarak)

3.  **Proses Simpul B (1)**:
    * Ambil `(1, B)` dari PQ.
    * Tetangga C: `dist[B] + 2 = 1 + 2 = 3`. Karena `3 < dist[C]` (yaitu `3 < 4`), `dist[C] = 3`, `parent[C] = B`. Masukkan `(3, C)` ke PQ.
    * Tetangga D: `dist[B] + 6 = 1 + 6 = 7`. Karena `7 < ∞`, `dist[D] = 7`, `parent[D] = B`. Masukkan `(7, D)` ke PQ.
    * `PQ = [(3, C), (4, C-lama), (7, D)]` (diurutkan)

4.  **Proses Simpul C (3)**:
    * Ambil `(3, C)` dari PQ. (Perhatikan `(4, C-lama)` masih ada di PQ tapi akan diabaikan karena `4 > dist[C]` yang sekarang 3).
    * Tetangga D: `dist[C] + 3 = 3 + 3 = 6`. Karena `6 < dist[D]` (yaitu `6 < 7`), `dist[D] = 6`, `parent[D] = C`. Masukkan `(6, D)` ke PQ.
    * `PQ = [(4, C-lama), (6, D-baru), (7, D-lama)]` (diurutkan)

5.  **Proses Simpul D (6)**:
    * Ambil `(6, D)` dari PQ.
    * Tidak ada tetangga yang jaraknya bisa diperbarui.

6.  **Sisa di PQ**: `(4, C-lama)` dan `(7, D-lama)` akan diambil tetapi diabaikan karena jaraknya lebih besar dari `dist` yang sudah final.

**Jarak Terpendek Final:**
* A: 0
* B: 1
* C: 3
* D: 6

**Rekonstruksi Jalur A → D (target D):**
* Dari D (index 3), parentnya C (index 2)
* Dari C (index 2), parentnya B (index 1)
* Dari B (index 1), parentnya A (index 0)
* Dari A (index 0), parentnya -1 (berhenti)
* Jalur: `D ← C ← B ← A`. Dibalik menjadi: `A → B → C → D`.

---

## 💻 Contoh Kode (C++)

```cpp
#include <iostream>    // Untuk input/output
#include <vector>      // Untuk std::vector
#include <queue>       // Untuk std::priority_queue
#include <climits>     // Untuk INT_MAX
#include <algorithm>   // Untuk std::reverse

using namespace std;

// typedef untuk membuat alias tipe data pair (jarak, node)
// Ini memudahkan penulisan dan pemahaman kode
typedef pair<int, int> pii; // Format: {jarak, node_id}

// Fungsi untuk menjalankan Algoritma Dijkstra
// V: jumlah total simpul (node) dalam graf
// adj: adjacency list yang merepresentasikan graf.
//      adj[u] berisi vector dari {neighbor_v, weight_u_v}
// source: simpul awal dari mana jalur terpendek akan dihitung
void dijkstra(int V, vector<vector<pii>>& adj, int source) {
    // vector dist: menyimpan jarak terpendek dari source ke setiap simpul
    // Inisialisasi semua jarak dengan INT_MAX (tak hingga)
    vector<int> dist(V, INT_MAX);
    
    // vector parent: menyimpan simpul sebelumnya di jalur terpendek
    // Digunakan untuk merekonstruksi jalur
    vector<int> parent(V, -1); // -1 menandakan tidak ada parent

    // priority_queue (min-heap): menyimpan pasangan {jarak_saat_ini, simpul}
    // Element dengan jarak terkecil akan selalu berada di puncak
    priority_queue<pii, vector<pii>, greater<pii>> pq;

    // Inisialisasi simpul awal
    dist[source] = 0;   // Jarak dari source ke source adalah 0
    pq.push({0, source}); // Masukkan simpul source ke priority queue

    // Proses utama Algoritma Dijkstra
    while (!pq.empty()) {
        int d = pq.top().first;  // Jarak yang diketahui ke simpul saat ini
        int u = pq.top().second; // Simpul yang sedang diproses
        pq.pop();                // Hapus dari priority queue

        // Jika jarak yang diambil dari PQ lebih besar dari jarak yang sudah tercatat
        // Artinya kita sudah menemukan jalur yang lebih pendek ke 'u' sebelumnya
        // Lewati iterasi ini (optimasi)
        if (d > dist[u]) {
            continue;
        }

        // Jelajahi semua tetangga dari simpul 'u'
        for (auto edge : adj[u]) {
            int v = edge.first;  // Simpul tetangga
            int w = edge.second; // Bobot sisi dari u ke v

            // Relaksasi: Jika jalur baru melalui 'u' lebih pendek ke 'v'
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;   // Perbarui jarak terpendek ke 'v'
                parent[v] = u;           // Set 'u' sebagai parent dari 'v'
                pq.push({dist[v], v});   // Masukkan 'v' ke priority queue dengan jarak baru
            }
        }
    }

    // --- Output Hasil ---

    // Menampilkan jarak minimum dari simpul sumber ke semua simpul lainnya
    cout << "Jarak minimum dari simpul " << char('A' + source) << ":\n";
    for (int i = 0; i < V; ++i) {
        cout << "Ke " << char('A' + i) << ": " << dist[i] << endl;
    }

    // Menampilkan jalur terpendek dari source ke simpul tertentu (misal: simpul D = 3)
    int target = 3; // Contoh: mencari jalur ke simpul D (indeks 3)
    cout << "\nJalur terpendek dari " << char('A' + source) << " ke " << char('A' + target) << ": ";
    
    // Jika target tidak terjangkau (jaraknya masih INT_MAX)
    if (dist[target] == INT_MAX) {
        cout << "Tidak dapat dijangkau." << endl;
    } else {
        vector<int> path;
        // Rekonstruksi jalur dengan backtracking dari target ke source menggunakan array parent
        for (int at = target; at != -1; at = parent[at]) {
            path.push_back(at);
        }
        // Karena jalur dibentuk dari target ke source, kita perlu membaliknya
        reverse(path.begin(), path.end());
        
        // Cetak jalur
        for (int i = 0; i < path.size(); ++i) {
            cout << char('A' + path[i]);
            if (i < path.size() - 1) {
                cout << " -> ";
            }
        }
        cout << endl;
    }
}

int main() {
    int V = 4; // Jumlah simpul dalam graf (A, B, C, D)

    // Deklarasi adjacency list. adj[i] akan berisi vector dari pair {neighbor_node, weight}
    vector<vector<pii>> adj(V);

    // Menambahkan sisi (edges) ke graf
    // Asumsi: A=0, B=1, C=2, D=3
    adj[0].push_back({1, 1}); // Sisi dari A (0) ke B (1) dengan bobot 1
    adj[0].push_back({2, 4}); // Sisi dari A (0) ke C (2) dengan bobot 4
    adj[1].push_back({2, 2}); // Sisi dari B (1) ke C (2) dengan bobot 2
    adj[1].push_back({3, 6}); // Sisi dari B (1) ke D (3) dengan bobot 6
    adj[2].push_back({3, 3}); // Sisi dari C (2) ke D (3) dengan bobot 3

    // Panggil fungsi Dijkstra dari simpul awal A (indeks 0)
    dijkstra(V, adj, 0); 

    return 0;
}
```

---

## 📚 Analisis Kompleksitas

### Kompleksitas Waktu

* **Inisialisasi**: $O(V)$ untuk mengisi `dist` dan `parent` arrays.
* **Proses Priority Queue**:
    * Setiap simpul (`V`) diambil dari `priority queue` sekali.
    * Setiap sisi (`E`) diproses sekali (relaksasi), dan setiap operasi `push` atau `decrease-key` pada `priority queue` memakan waktu $O(\log V)$.
* **Total Kompleksitas Waktu**: $O((V + E) \log V)$.
    * Jika graf padat ($E \approx V^2$), menjadi $O(V^2 \log V)$.
    * Jika graf jarang ($E \approx V$), menjadi $O(V \log V)$.
    * Untuk implementasi dengan *Fibonacci heap* (lebih kompleks), bisa mencapai $O(E + V \log V)$, yang lebih cepat untuk graf padat. Namun, *binary heap* (`std::priority_queue`) adalah yang paling umum dan praktis.

### Kompleksitas Ruang

* **Adjacency List**: $O(V + E)$ untuk menyimpan struktur graf.
* **`dist` dan `parent` arrays**: Masing-masing $O(V)$.
* **Priority Queue**: Dalam kasus terburuk, bisa menyimpan hingga $O(E)$ elemen (jika banyak simpul ditambahkan dan diperbarui sebelum diambil) atau $O(V)$ elemen (jika setiap simpul hanya ditambahkan sekali).

**Total Kompleksitas Ruang**: $O(V + E)$.

---

## 🌟 Aplikasi Dunia Nyata

Algoritma Dijkstra adalah tulang punggung dari banyak sistem penting:

* **Navigasi GPS dan Aplikasi Peta**: Seperti Google Maps, Waze, atau Grab. Digunakan untuk menemukan rute tercepat atau terpendek antar lokasi.
* **Rute Terpendek dalam *Game***: AI karakter non-pemain (NPC) sering menggunakan Dijkstra untuk menemukan jalur optimal di lingkungan *game*.
* **Jaringan Komunikasi dan Routing**: Dalam router jaringan, untuk menentukan jalur terbaik bagi paket data agar mencapai tujuan dengan latensi terendah atau *hop* paling sedikit.
* **Perencanaan Biaya Minimum**: Dalam transportasi dan logistik, untuk mengoptimalkan rute pengiriman dengan biaya atau waktu terendah.
* **Analisis Jarak Sosial/Jaringan Relasi**: Menemukan "derajat perpisahan" minimum antara dua orang dalam jaringan sosial.

---

## 💪 Kekuatan dan Keterbatasan

### Kekuatan

* **Menjamin Jalur Terpendek**: Algoritma ini bersifat *optimal* dan dijamin akan menemukan jalur terpendek dari satu simpul sumber ke semua simpul lain, asalkan bobot sisinya non-negatif.
* **Efisien untuk Banyak Aplikasi**: Dengan implementasi menggunakan *priority queue*, kinerjanya sangat baik untuk graf berukuran menengah hingga besar.
* **Dapat Menyelesaikan Banyak Tujuan Sekaligus**: Dalam satu kali eksekusi dari simpul sumber, Dijkstra menghitung jalur terpendek ke *semua* simpul lain.

### Keterbatasan

* **Tidak Bekerja dengan Bobot Negatif**: Jika ada sisi dengan bobot negatif, Dijkstra bisa memberikan hasil yang salah. Untuk kasus ini, algoritma seperti Bellman-Ford atau Floyd-Warshall lebih cocok.
* **Kurang Efisien untuk Graf Sangat Besar yang Jarang Terhubung**: Meskipun $O((V+E)\log V)$ efisien, untuk graf yang sangat besar dan sangat jarang, algoritma lain mungkin lebih cepat jika hanya perlu menemukan jalur ke satu tujuan tertentu (misalnya, A*).
* **Perhitungan Bisa Terlalu Luas**: Jika hanya perlu jalur terpendek ke satu simpul tertentu, Dijkstra akan tetap menghitung jarak terpendek ke semua simpul lain yang bisa dijangkau, yang mungkin tidak selalu efisien.

---

## 🏁 Kesimpulan

**Algoritma Dijkstra** merupakan salah satu algoritma pencarian jalur terpendek yang paling efisien dan banyak digunakan dalam teori graf. Algoritma ini bekerja dengan cara mencari jalur terpendek dari satu simpul sumber ke semua simpul lainnya dalam **graf berbobot non-negatif**. Melalui pendekatan *greedy* yang cermat, Dijkstra memastikan bahwa setiap langkahnya mendekatkan solusi menuju hasil optimal. Keunggulan utama algoritma ini terletak pada keakuratannya dan efisiensinya, terutama jika dikombinasikan dengan struktur data seperti **priority queue** atau **min-heap**. Dalam implementasi praktis, Algoritma Dijkstra adalah alat yang sangat berguna dalam berbagai bidang seperti jaringan komputer, sistem navigasi, dan pemodelan transportasi.

---