---
title: ACTIVITY SELECTION PROBLEM
date: 2025-05-06 08:00:00 +0800
categories: [DESAIN DAN ANALISIS ALGORITMA, GREEDY ALGORITHM]
tags: [daa, algorithm, greedy, c++]
---

![Desktop View](/assets/lib/images/post/ASP.png)

# Memaksimalkan Jadwal: Memahami Activity Selection Problem

## 🎯 Pendahuluan: Mengapa Memilih Aktivitas dengan Cerdas?

Bayangkan Anda punya daftar kegiatan dengan waktu mulai dan selesai yang berbeda. Tujuan Anda? Memilih **sebanyak mungkin kegiatan** yang bisa dilakukan tanpa ada yang **tumpang tindih**. Inilah inti dari **Activity Selection Problem**, sebuah tantangan optimasi klasik dalam ilmu komputer!

---
## 🧠 Konsep Inti: Strategi "Greedy" dalam Pemilihan Aktivitas

### Apa itu Activity Selection Problem?

Masalah ini berfokus pada pemilihan **set aktivitas terbesar yang saling kompatibel** dari sekumpulan aktivitas yang diberikan, di mana setiap aktivitas punya waktu mulai ($s$) dan waktu selesai ($f$). Dua aktivitas dikatakan kompatibel jika satu aktivitas selesai sebelum aktivitas lainnya dimulai.

### Mengapa Greedy Algorithm?

**Algoritma Greedy** adalah pendekatan pemecahan masalah optimasi yang membuat pilihan **terbaik pada setiap langkah saat ini**, dengan harapan bahwa serangkaian pilihan lokal ini akan mengarah pada solusi optimal secara keseluruhan. Dalam konteks pemilihan aktivitas, strategi "serakah" kita adalah: **selalu pilih aktivitas yang selesai paling awal.**

Strategi ini bekerja karena dengan membebaskan sumber daya secepat mungkin, kita memberikan peluang maksimal bagi aktivitas-aktivitas lain untuk dijadwalkan setelahnya.

---

## 🧑‍💻 Bagaimana Algoritma Bekerja?

### Data yang Dibutuhkan

* **Input**: Dua array (atau struktur data serupa) berisi waktu mulai ($s$) dan waktu selesai ($f$) untuk setiap aktivitas.
* **Output**: Indeks dari aktivitas-aktivitas yang dipilih yang tidak tumpang tindih.

### Langkah-langkah Solusi

1.  **Urutkan Aktivitas**: Kunci utama algoritma ini adalah mengurutkan semua aktivitas berdasarkan **waktu selesai (finish time)** secara menaik.
2.  **Pilih Aktivitas Pertama**: Setelah diurutkan, aktivitas pertama (yang selesai paling awal) akan selalu jadi bagian dari solusi optimal. Pilih aktivitas ini.
3.  **Iterasi dan Pilih yang Kompatibel**: Mulai dari aktivitas yang baru saja dipilih, cari aktivitas berikutnya dalam daftar yang waktu mulainya **lebih besar atau sama** dengan waktu selesai dari aktivitas terakhir yang dipilih. Ulangi proses ini sampai semua aktivitas sudah diperiksa.

---

## 💡 Contoh Studi Kasus

Mari kita lihat contoh untuk memahami langkah-langkahnya:

**Daftar Aktivitas:**

| Aktivitas | Mulai (s) | Selesai (f) |
| :-------- | :-------- | :---------- |
| A1        | 1         | 4           |
| A2        | 3         | 5           |
| A3        | 0         | 6           |
| A4        | 5         | 7           |
| A5        | 8         | 9           |
| A6        | 5         | 9           |

**Langkah Solusi:**

1.  **Urutkan berdasarkan Waktu Selesai**:
    * A1: (1, 4)
    * A2: (3, 5)
    * A3: (0, 6)
    * A4: (5, 7)
    * A5: (8, 9)
    * A6: (5, 9)

2.  **Pilih A1**: Aktivitas pertama yang selesai paling awal.
    * Solusi sementara: `{A1}`. Waktu selesai terakhir: 4.

3.  **Iterasi**:
    * Cari aktivitas yang dimulai pada atau setelah waktu 4.
    * A2 (3, 5): Tumpang tindih dengan A1. Lewati.
    * A3 (0, 6): Tumpang tindih dengan A1. Lewati.
    * A4 (5, 7): **Kompatibel!** Dimulai pada 5, yang $\ge$ 4.
        * Solusi sementara: `{A1, A4}`. Waktu selesai terakhir: 7.
    * A5 (8, 9): **Kompatibel!** Dimulai pada 8, yang $\ge$ 7.
        * Solusi sementara: `{A1, A4, A5}`. Waktu selesai terakhir: 9.
    * A6 (5, 9): Tumpang tindih dengan A5. Lewati.

**Solusi Optimal**: `{A1, A4, A5}` (total 3 aktivitas dapat dipilih).

---

## 💻 Implementasi dalam C++

```cpp
#include <iostream>
#include <vector>
#include <algorithm> // Untuk fungsi sort
using namespace std;

// Struktur untuk merepresentasikan setiap aktivitas
struct Activity {
    int start, finish, index; // Waktu mulai, waktu selesai, dan indeks asli
};

// Fungsi komparator untuk mengurutkan aktivitas berdasarkan waktu selesai
bool compare(Activity a, Activity b) {
    return a.finish < b.finish;
}

// Fungsi utama untuk memilih aktivitas
void activitySelection(vector<Activity>& activities) {
    // Urutkan aktivitas menggunakan komparator
    sort(activities.begin(), activities.end(), compare);

    cout << "Aktivitas terpilih (berdasarkan indeks asli): ";
    
    // Inisialisasi waktu selesai dari aktivitas pertama yang dipilih
    int lastFinish = -1; 

    // Iterasi melalui aktivitas yang sudah diurutkan
    for (auto act : activities) {
        // Jika waktu mulai aktivitas saat ini >= waktu selesai aktivitas terakhir yang dipilih
        if (act.start >= lastFinish) {
            cout << act.index << " "; // Cetak indeks aktivitas yang dipilih
            lastFinish = act.finish;  // Perbarui waktu selesai terakhir
        }
    }
    cout << endl;
}

int main() {
    // Contoh data aktivitas
    vector<Activity> activities = {
        {1, 4, 0}, // A1 (indeks 0)
        {3, 5, 1}, // A2 (indeks 1)
        {0, 6, 2}, // A3 (indeks 2)
        {5, 7, 3}, // A4 (indeks 3)
        {8, 9, 4}, // A5 (indeks 4)
        {5, 9, 5}  // A6 (indeks 5)
    };

    // Panggil fungsi pemilihan aktivitas
    activitySelection(activities);
    return 0;
}
```

---

## 📚 Analisis Kinerja

### Kompleksitas Waktu

* **Pengurutan**: Tahap pengurutan aktivitas berdasarkan waktu selesai mendominasi kompleksitas waktu, yaitu $O(n \log n)$, di mana $n$ adalah jumlah aktivitas.
* **Pemilihan**: Proses iterasi dan pemilihan aktivitas yang kompatibel hanya memerlukan $O(n)$ waktu.
* **Total**: Kompleksitas waktu keseluruhan adalah $O(n \log n)$.

### Kompleksitas Ruang

* Algoritma ini memerlukan $O(n)$ ruang untuk menyimpan aktivitas. Tidak ada ruang tambahan signifikan yang diperlukan selain untuk input dan output.

---

## 🌟 Aplikasi di Dunia Nyata

Meskipun sederhana, Activity Selection Problem punya banyak aplikasi praktis:

* **Penjadwalan Ruangan/Fasilitas**: Mengatur penggunaan ruang kelas, ruang rapat, laboratorium, atau lapangan olahraga agar sebanyak mungkin kegiatan bisa dilakukan.
* **Sistem Operasi**: Penjadwalan proses CPU atau alokasi sumber daya memori untuk memaksimalkan *throughput*.
* **Logistik**: Penjadwalan pengiriman barang atau rute kendaraan untuk efisiensi maksimal.
* **Telekomunikasi**: Alokasi *bandwidth* atau penjadwalan transmisi data.

---

## 💪 Kekuatan dan Keterbatasan

### Kekuatan

* **Sederhana**: Konsepnya mudah dipahami dan diimplementasikan.
* **Efisien**: Dengan kompleksitas $O(n \log n)$, sangat cocok untuk dataset besar.
* **Optimal**: Memberikan solusi optimal untuk masalah pemilihan aktivitas tanpa batasan tambahan.

### Keterbatasan

* **Bergantung pada Pengurutan**: Membutuhkan langkah pengurutan awal yang dapat jadi *bottleneck* untuk data yang sangat besar jika tidak dioptimalkan.
* **Tidak Cocok untuk Batasan Kompleks**: Jika ada faktor tambahan seperti prioritas aktivitas, biaya, atau jarak antar lokasi, algoritma *greedy* sederhana ini mungkin tidak lagi optimal. Dalam kasus tersebut, pendekatan lain seperti pemrograman dinamis atau metaheuristik mungkin diperlukan.

---

## 🏁 Kesimpulan

**Activity Selection Problem** adalah contoh luar biasa bagaimana Algoritma Greedy, dengan strategi sederhana namun efektif (memilih yang selesai paling awal), dapat menghasilkan solusi optimal secara efisien. Meskipun memiliki keterbatasan untuk skenario yang lebih kompleks, konsepnya tetap menjadi dasar penting dalam pemecahan masalah penjadwalan dan optimasi.

---