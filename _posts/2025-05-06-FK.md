---
title: FRACTIONAL KNAPSACK
date: 2025-05-06 08:00:00 +0800
categories: [DESAIN ANALISIS ALGORITMA, GREEDY ALGORITHM]
tags: [daa, algorithm, greedy, c++]
---

![Desktop View](/assets/images/post/FK.png)

# Mengoptimalkan Muatan: Memahami Fractional Knapsack

## 🎒 Mengenal Fractional Knapsack

### Apa Itu Knapsack Problem?

**Knapsack Problem** adalah tantangan optimasi di mana kita harus memilih item dari sekumpulan pilihan untuk memaksimalkan nilai total, tanpa melebihi kapasitas berat ransel yang tersedia. Ada dua variasi utama:

1.  **0/1 Knapsack**: Anda harus memilih **seluruh item atau tidak sama sekali**. Item tidak bisa dipotong. Ini biasanya diselesaikan dengan pemrograman dinamis karena kompleksitasnya.
2.  **Fractional Knapsack**: Ini adalah fokus kita! Anda **diperbolehkan mengambil sebagian dari item** (memotongnya) untuk memaksimalkan nilai. Perbedaan ini memungkinkan kita menggunakan pendekatan yang lebih sederhana dan efisien: **Algoritma Greedy**.

---

## 🧠 Konsep Inti: Greedy dalam Fractional Knapsack

### Filosofi Algoritma Greedy

**Algoritma Greedy** adalah strategi pemecahan masalah yang membuat pilihan yang tampak terbaik pada setiap langkah, berharap bahwa serangkaian pilihan lokal ini akan mengarahkan pada solusi optimal secara keseluruhan. Algoritma ini "serakah" karena langsung mengambil opsi terbaik saat itu tanpa mempertimbangkan konsekuensi jangka panjang.

Dalam konteks **Fractional Knapsack**, pendekatan *greedy* bekerja dengan sangat baik. Pilihan terbaik lokal (mengambil item dengan rasio nilai per berat tertinggi) secara konsisten mengarah pada solusi optimal global.

### Karakteristik Utama:

* Setiap barang memiliki **berat** dan **nilai**.
* Barang **dapat diambil sebagian** sesuai kebutuhan.
* Solusi dapat ditemukan secara **optimal** menggunakan algoritma *greedy*.

---

## 🧑‍💻 Bagaimana Algoritma Bekerja?

### Data yang Dibutuhkan

* **Input**:
    * `n`: Jumlah total item.
    * `W`: Kapasitas maksimum ransel (dalam satuan berat).
    * Daftar item, masing-masing dengan:
        * `value[i]`: Nilai item ke-i.
        * `weight[i]`: Berat item ke-i.
* **Output**:
    * **Total nilai maksimum** yang bisa dibawa (max\_value).
    * (Opsional) Detail persentase atau bagian item yang diambil.

### Langkah-langkah Penyelesaian

1.  **Hitung Rasio Nilai per Berat**: Untuk setiap item, hitung rasio `nilai / berat` (`value[i] / weight[i]`). Rasio ini menunjukkan "kepadatan" nilai item tersebut.
2.  **Urutkan Barang**: Urutkan semua item dalam urutan menurun berdasarkan **rasio nilai per berat** yang telah dihitung.
3.  **Isi Ransel**: Mulai dari item dengan rasio tertinggi, ambil sebanyak mungkin dari item tersebut hingga ransel penuh atau item habis.
    * Jika item muat seluruhnya, ambil semua dan kurangi kapasitas ransel.
    * Jika item tidak muat seluruhnya, ambil sebagian (fraksi) yang dapat memenuhi sisa kapasitas ransel. Hitung nilai fraksional yang diperoleh dan hentikan proses.

---

## 💡 Contoh Studi Kasus

**Diberikan 4 item dengan data:**

| Item | Nilai (Value) | Berat (Weight) |
| :--- | :------------ | :------------- |
| A    | 50            | 10             |
| B    | 60            | 20             |
| C    | 140           | 40             |
| D    | 60            | 30             |

Kapasitas ransel (`W`) = 50

**Pertanyaan**: Berapa **nilai maksimum** yang bisa dibawa dalam tas?

**Langkah Solusi:**

1.  **Hitung Rasio `Value/Weight`**:
    * A: $50 / 10 = 5.0$
    * B: $60 / 20 = 3.0$
    * C: $140 / 40 = 3.5$
    * D: $60 / 30 = 2.0$

2.  **Urutkan berdasarkan Rasio (menurun)**:
    * A (5.0)
    * C (3.5)
    * B (3.0)
    * D (2.0)

3.  **Isi Ransel berdasarkan Kapasitas**:
    * **Ambil Item A**: Berat 10 kg, Nilai 50.
        * Sisa kapasitas ransel: $50 - 10 = 40$ kg.
        * Total nilai sementara: 50.
    * **Ambil Item C**: Berat 40 kg, Nilai 140.
        * Sisa kapasitas ransel: $40 - 40 = 0$ kg.
        * Total nilai sementara: $50 + 140 = 190$.
    * Ransel sudah penuh (kapasitas 0). Item B dan D tidak diambil.

**Total Nilai Maksimum = 190.**

---

## 💻 Contoh Kode (C++)

```cpp
#include <iostream>
#include <vector>
#include <algorithm> // Untuk fungsi sort

using namespace std;

// Struktur untuk merepresentasikan setiap item
struct Item {
    string name;  // Nama item (opsional, untuk tampilan)
    double weight; // Berat item
    double value;  // Nilai item

    // Fungsi untuk menghitung rasio nilai per berat
    double ratio() const {
        return value / weight;
    }
};

// Fungsi komparator untuk mengurutkan item berdasarkan rasio (dari terbesar ke terkecil)
bool compare(Item a, Item b) {
    return a.ratio() > b.ratio(); // Urutkan secara menurun berdasarkan rasio
}

int main() {
    double capacity = 30.0; // Kapasitas maksimum ransel
    
    // Daftar item yang tersedia
    vector<Item> items = {
        {"Laptop", 10, 300},
        {"Buku Paket", 20, 200},
        {"Baju", 30, 180}
    };

    // Urutkan item berdasarkan rasio nilai/berat
    sort(items.begin(), items.end(), compare);

    double totalValue = 0.0; // Variabel untuk menyimpan total nilai
    
    cout << "Barang yang dipilih:\n";
    for (const auto& item : items) {
        // Hentikan jika kapasitas ransel sudah habis
        if (capacity == 0) break;

        // Jika item saat ini bisa diambil seluruhnya
        if (item.weight <= capacity) {
            totalValue += item.value;      // Tambahkan nilai item
            capacity -= item.weight;       // Kurangi kapasitas ransel
            cout << "- " << item.name << " (berat: " << item.weight << " kg, nilai: " << item.value << ")\n";
        } else {
            // Jika item hanya bisa diambil sebagian (fraksi)
            double fraction = capacity / item.weight; // Hitung fraksi yang bisa diambil
            totalValue += item.value * fraction;      // Tambahkan nilai fraksional
            cout << "- " << item.name << " (ambil " << capacity << " kg dari " << item.weight << " kg, nilai: " << item.value * fraction << ")\n";
            capacity = 0; // Kapasitas ransel menjadi nol
        }
    }

    cout << "\nTotal nilai yang dibawa: " << totalValue << endl;

    return 0;
}
```

---

## 📚 Analisis Kompleksitas

### Kompleksitas Waktu

* **Menghitung rasio `value/weight`**: Dilakukan untuk setiap item, sehingga $O(n)$.
* **Mengurutkan item**: Proses pengurutan mendominasi, yaitu $O(n \log n)$.
* **Mengisi knapsack**: Iterasi melalui item yang sudah diurutkan, proses ini $O(n)$.

**Total kompleksitas waktu**: $O(n \log n)$.

### Kompleksitas Ruang

* Penyimpanan item (berat, nilai, rasio) memerlukan $O(n)$ ruang.
* Tidak ada penggunaan struktur data tambahan yang kompleks atau rekursi yang signifikan.

**Total kompleksitas ruang**: $O(n)$.

---

## 🌟 Aplikasi di Dunia Nyata

Fractional Knapsack punya banyak penerapan praktis:

* **Pengangkutan Barang Logistik**: Memilih barang yang akan dimuat ke dalam truk atau kapal dengan kapasitas terbatas untuk memaksimalkan nilai total muatan.
* **Pengisian Tangki/Wadah**: Mengisi tangki minyak atau bahan bakar dengan campuran produk yang berbeda untuk memaksimalkan keuntungan.
* **Investasi Portofolio**: Mengalokasikan dana ke berbagai aset (saham, obligasi) untuk memaksimalkan *return* berdasarkan batas anggaran.
* **Penggunaan Sumber Daya Komputasi**: Mengalokasikan *bandwidth* jaringan atau *CPU time* di *cloud computing* untuk berbagai tugas agar penggunaan sumber daya optimal.

---

## 💪 Kekuatan dan Keterbatasan

### Kekuatan

* **Optimalitas Terjamin**: Untuk masalah Fractional Knapsack, algoritma *greedy* **menjamin solusi optimal**.
* **Cepat dan Efisien**: Kompleksitas waktu $O(n \log n)$ membuatnya sangat efisien, bahkan untuk dataset besar.
* **Fleksibel**: Mudah diimplementasikan dan dimodifikasi.

### Kekurangan

* **Tidak Cocok untuk 0/1 Knapsack**: Algoritma ini **tidak dapat digunakan** untuk varian 0/1 Knapsack, di mana item tidak boleh dipotong.
* **Fokus Tunggal**: Hanya memaksimalkan nilai berdasarkan berat, tidak mempertimbangkan batasan lain seperti prioritas, risiko, atau ketergantungan antar item.

---

## 🏁 Kesimpulan

**Fractional Knapsack** adalah salah satu variasi dari **Knapsack Problem** yang dapat diselesaikan secara efisien dan optimal menggunakan **algoritma *greedy***. Kunci efektivitasnya adalah kemampuan untuk mengambil **sebagian (fraksional)** dari item, memungkinkan kita fokus pada **rasio nilai terhadap berat** (`value/weight`) tertinggi. Dengan mengurutkan item berdasarkan rasio ini dan mengambilnya secara berurutan, kita dapat dengan cepat mencapai nilai total maksimum yang bisa dimuat ke dalam ransel. Pendekatan ini menunjukkan bagaimana pilihan "serakah" lokal yang tepat dapat menghasilkan solusi global yang sempurna.

---