---
title: HUFFMAN CODING
date: 2025-05-20 08:00:00 +0800
categories: [DESAIN ANALISIS ALGORITMA, GREEDY ALGORITHM]
tags: [daa, algorithm, greedy]
---

![Desktop View](/assets/images/post/HUFFMAN.png)

# Mengoptimalkan Data: Memahami Huffman Coding

## 🧾 Pengantar: Mengapa Kita Perlu Kompresi Data ?

Pernahkah Anda bertanya bagaimana file bisa menjadi lebih kecil tanpa kehilangan informasinya? Jawabannya ada pada algoritma kompresi! Salah satu yang paling elegan dan efisien adalah **Huffman Coding**.

**Huffman Coding** adalah algoritma **kompresi data *lossless*** yang dikembangkan oleh David A. Huffman pada tahun 1952. Tujuannya sederhana namun powerful: **mengurangi ukuran data** dengan mengganti simbol yang sering muncul dengan kode bit yang lebih pendek. Algoritma ini jadi fondasi banyak format kompresi modern, seperti **ZIP, JPEG, dan MP3**.

---

## 🧠 Konsep Inti: Kode Variabel Berdasarkan Frekuensi

### Bagaimana Huffman Coding Bekerja ? 

Inti dari Huffman Coding terletak pada prinsip **kode panjang variabel**. Ini berarti setiap karakter tidak memiliki panjang kode bit yang sama.

* **Frekuensi Karakter**: Algoritma ini sangat bergantung pada frekuensi kemunculan setiap karakter dalam data.
* **Kode Pendek untuk Sering Muncul**: Karakter yang sering muncul akan diberi kode bit yang **lebih pendek**.
* **Kode Panjang untuk Jarang Muncul**: Sebaliknya, karakter yang jarang muncul akan diberi kode bit yang **lebih panjang**.
* **Pohon Biner**: Representasi data ini dibangun menggunakan struktur **pohon biner** khusus yang disebut Pohon Huffman.

---

## 🧑‍💻 Proses Pembentukan Kode Huffman

Pembentukan kode Huffman melibatkan beberapa langkah sistematis:

1.  **Hitung Frekuensi**: Mulai dengan menghitung frekuensi (jumlah kemunculan) setiap karakter unik dalam data yang akan dikompresi.
2.  **Buat Simpul (Node)**: Setiap karakter dengan frekuensinya akan menjadi sebuah simpul daun (leaf node).
3.  **Gabungkan Simpul Terkecil**: Tempatkan semua simpul ke dalam *min-priority queue*. Ambil dua simpul dengan frekuensi terkecil, gabungkan menjadi simpul induk baru (dengan frekuensi total dari kedua simpul anak), lalu masukkan kembali simpul induk ini ke dalam *queue*.
4.  **Ulangi Proses**: Terus ulangi langkah 3 sampai hanya ada satu simpul tersisa di *queue*. Simpul ini akan menjadi akar dari Pohon Huffman.
5.  **Tentukan Kode Biner**: Telusuri Pohon Huffman dari akar ke setiap simpul daun. Beri label "0" untuk setiap cabang kiri dan "1" untuk setiap cabang kanan. Jalur dari akar ke simpul daun akan membentuk kode biner unik untuk karakter tersebut.

---

## 📥 Input & 📤 Output

### Input

Biasanya Huffman Coding menerima salah satu dari ini:

1.  **Daftar Karakter dan Frekuensinya**:
    * Contoh: `Karakter: [a, b, c, d, e, f]`, `Frekuensi: [5, 9, 12, 13, 16, 45]`
2.  **String (Teks) Mentah**: Algoritma akan menghitung frekuensinya secara otomatis.
    * Contoh: `Input: "beep boop beer!"`

### Output

Setelah proses kompresi, Anda akan mendapatkan:

1.  **Kode Huffman untuk Setiap Karakter**: Daftar pasangan `karakter : kode biner`.
2.  **String Terenkripsi (Encoded Message)**: Representasi biner dari data asli yang sudah dikompres.
3.  **Pohon Huffman (Opsional)**: Struktur pohon yang digunakan untuk encoding dan decoding.

---

## 🧮 Langkah Solusi & Contoh

Mari kita gunakan contoh untuk memahami prosesnya:

**Contoh: String = "ABBCCCDDDD"**

1.  **Hitung Frekuensi Karakter**:
    * A: 1
    * B: 2
    * C: 3
    * D: 4

2.  **Bangun Pohon Huffman berdasarkan frekuensi**:
    * (Ini adalah proses iteratif menggabungkan simpul terkecil)
    * Misal, A(1) dan B(2) digabung jadi AB(3).
    * AB(3) dan C(3) digabung jadi ABC(6).
    * ABC(6) dan D(4) digabung jadi ABCD(10) (akar).

3.  **Buat Kode Huffman**:
    * D = 0
    * C = 10
    * B = 110
    * A = 111

4.  **Enkode String**:
    * "ABBCCCDDDD" akan menjadi:
        * A: 111
        * B: 110
        * B: 110
        * C: 10
        * C: 10
        * C: 10
        * D: 0
        * D: 0
        * D: 0
        * D: 0
    * **Output Terkompresi**: `111110110101010000000`
    * Perhatikan bagaimana representasi ini jauh lebih pendek daripada jika setiap karakter menggunakan 8 bit (ASCII standar), atau bahkan 3 bit (misal, A=000, B=001, C=010, D=011), karena D yang paling sering muncul hanya menggunakan 1 bit!

---

## 💻 Contoh Kode (C++)

```cpp
#include <iostream>
#include <queue>         // Untuk priority_queue
#include <unordered_map> // Untuk menyimpan frekuensi dan kode
#include <vector>        // Digunakan oleh priority_queue

using namespace std;

// Struktur Node untuk pohon Huffman
struct Node {
    char ch;  // Karakter yang disimpan di node ini
    int freq; // Frekuensi karakter
    Node *left, *right; // Pointer ke anak kiri dan kanan

    // Konstruktor Node
    Node(char c, int f) {
        ch = c;
        freq = f;
        left = right = nullptr; // Inisialisasi anak sebagai null
    }
};

// Komparator untuk priority_queue (membuat min-heap berdasarkan frekuensi)
struct Compare {
    bool operator()(Node* a, Node* b) {
        return a->freq > b->freq; // Prioritas lebih rendah untuk frekuensi lebih kecil
    }
};

// Fungsi rekursif untuk menghasilkan kode Huffman
void generateCodes(Node* root, string code, unordered_map<char, string>& huffmanCode) {
    // Jika node null, kembali
    if (!root) return;

    // Jika ini adalah node daun (tidak punya anak), maka ini adalah karakter
    if (!root->left && !root->right) {
        huffmanCode[root->ch] = code; // Simpan kode Huffman untuk karakter ini
    }

    // Telusuri anak kiri, tambahkan '0' ke kode
    generateCodes(root->left, code + "0", huffmanCode);
    // Telusuri anak kanan, tambahkan '1' ke kode
    generateCodes(root->right, code + "1", huffmanCode);
}

// Fungsi utama Huffman Coding
void huffmanCoding(const unordered_map<char, int>& freqMap) {
    // Buat min-heap (priority_queue) untuk menyimpan node
    priority_queue<Node*, vector<Node*>, Compare> minHeap;

    // Masukkan setiap karakter dan frekuensinya sebagai node ke dalam min-heap
    for (auto pair : freqMap) {
        minHeap.push(new Node(pair.first, pair.second));
    }

    // Bangun pohon Huffman
    // Lanjutkan selama ada lebih dari satu node di heap
    while (minHeap.size() > 1) {
        // Ambil dua node dengan frekuensi terkecil
        Node* left = minHeap.top(); minHeap.pop();
        Node* right = minHeap.top(); minHeap.pop();

        // Buat node induk baru dengan frekuensi gabungan
        // '\0' digunakan sebagai placeholder karena node internal tidak mewakili karakter
        Node* merged = new Node('\0', left->freq + right->freq);
        merged->left = left;  // Node kiri adalah anak kiri
        merged->right = right; // Node kanan adalah anak kanan

        minHeap.push(merged); // Masukkan node induk kembali ke heap
    }

    // Node terakhir di heap adalah akar pohon Huffman
    Node* root = minHeap.top();

    // Peta untuk menyimpan kode Huffman yang dihasilkan
    unordered_map<char, string> huffmanCode;
    // Panggil fungsi rekursif untuk menghasilkan kode
    generateCodes(root, "", huffmanCode);

    cout << "Kode Huffman untuk tiap karakter:\n";
    for (auto pair : huffmanCode) {
        cout << pair.first << " : " << pair.second << endl;
    }

    // Penting: Dealokasikan memori untuk node pohon
    // (Dalam contoh sederhana ini diabaikan untuk fokus pada algoritma inti)
}

int main() {
    // Contoh Peta Frekuensi (bisa didapatkan dari input string)
    unordered_map<char, int> freqMap = {
        {'a', 5}, {'b', 9}, {'c', 12},
        {'d', 13}, {'e', 16}, {'f', 45}
    };

    huffmanCoding(freqMap); // Panggil fungsi Huffman Coding

    return 0;
}
```

---

## 📚 Analisis Kompleksitas

### Kompleksitas Waktu

* **Membangun Pohon Huffman**: Ini adalah bagian yang paling dominan. Dengan `n` karakter unik, kita melakukan $n-1$ kali operasi `insert` dan `extract-min` pada *min-heap*. Setiap operasi ini memerlukan $O(\log n)$ waktu. Jadi, total waktu untuk membangun pohon adalah $O(n \log n)$.
* **Traversal Pohon**: Untuk menghasilkan kode Huffman, kita melakukan traversal DFS pada pohon. Ini memerlukan $O(n)$ waktu untuk $n$ simpul daun (karakter unik).
* **Encoding String (jika input berupa teks)**: Jika panjang teks adalah $L$, proses encoding akan membutuhkan $O(L)$ waktu, karena setiap karakter hanya perlu dicari kodenya (yang sudah disimpan dalam *map*) dan digabungkan.

**Total kompleksitas waktu**: $O(n \log n + L)$ (dengan $n$ adalah jumlah karakter unik dan $L$ adalah panjang string asli).

### Kompleksitas Ruang

* **Penyimpanan Node Pohon**: Pohon Huffman akan memiliki $n$ simpul daun dan $n-1$ simpul internal, total $2n-1$ simpul. Ini memerlukan $O(n)$ ruang.
* **Peta Kode Huffman**: Untuk setiap karakter unik, kita menyimpan kode binernya. Dalam kasus terburuk, panjang kode bisa mencapai $O(n)$ (untuk pohon yang sangat miring). Jadi, total ruang untuk menyimpan kode adalah $O(n \times \text{panjang kode maks})$, yang bisa mencapai $O(n^2)$ dalam kasus ekstrem (namun jarang terjadi di praktiknya), atau $O(n \log n)$ rata-rata.
* **String Terenkripsi**: Panjangnya bisa mencapai maksimal $L \times \text{panjang kode maks}$.

**Total kompleksitas ruang**: $O(n + L)$ (untuk menyimpan pohon dan hasil string terenkripsi).

---

## 🌟 Aplikasi Dunia Nyata

Huffman Coding ada di mana-mana! Beberapa aplikasinya meliputi:

* **Kompresi File**: Fondasi dari format kompresi populer seperti **ZIP, GZIP, dan 7z**.
* **Format Gambar**: Digunakan dalam kompresi *lossless* pada format seperti **JPEG** (untuk data DC coefficients) dan **PNG**.
* **Format Video dan Audio**: Berperan dalam standar seperti **MP3, MPEG, dan H.264** untuk kompresi bagian data tertentu.
* **Transmisi Data**: Membantu mengurangi *bandwidth* yang diperlukan untuk transmisi data melalui jaringan telekomunikasi dan satelit.
* **Sistem Informasi & Penyimpanan Dokumen**: Efisien untuk menyimpan arsip teks dan dokumen lainnya.

---

## 💪 Kelebihan dan Kekurangan

### Kelebihan

* **Lossless**: Tidak ada data yang hilang selama proses kompresi dan dekompresi, memastikan integritas informasi.
* **Efisien untuk Distribusi Tidak Merata**: Sangat efisien jika frekuensi kemunculan karakter dalam data tidak merata (ada karakter yang sering muncul dan jarang muncul).
* **Digunakan Luas**: Merupakan algoritma dasar yang diterapkan di banyak sistem kompresi file dan multimedia.

### Kekurangan

* **Kurang Optimal jika Frekuensi Merata**: Jika semua karakter memiliki frekuensi kemunculan yang hampir sama, efisiensi kompresinya akan menurun.
* **Memerlukan Tabel Kode**: Saat mendekode data, kita memerlukan tabel kode Huffman yang sama yang digunakan saat encoding. Tabel ini harus disimpan atau dikirim bersama data terkompresi, menambah sedikit *overhead*.

---

## 🏁 Kesimpulan

**Huffman Coding** adalah salah satu algoritma **kompresi data *lossless*** paling efisien dan banyak digunakan di berbagai aplikasi dunia nyata. Dengan pendekatan berbasis frekuensi karakter, algoritma ini secara cerdas menghasilkan representasi biner yang lebih pendek untuk simbol yang sering muncul, sehingga mampu mengurangi ukuran data secara signifikan tanpa kehilangan informasi. Ini adalah contoh klasik dari aplikasi algoritma *greedy* yang menghasilkan solusi optimal secara global.

---