---
title: SUBSET SUM PROBLEM
date: 2025-05-20 08:00:00 +0800
categories: [DESAIN ANALISIS ALGORITMA, BACKTRACKING ALGORITHM]
tags: [daa, algorithm, backtracking, c++]
---

![Desktop View](/assets/images/post/SSP.png)

# Mengurai Angka: Memecahkan Subset Sum Problem

## 🧨 Pengantar: Mencari Kombinasi yang Tepat

Pernahkah Anda dihadapkan pada sekumpulan angka dan diminta mencari tahu apakah beberapa di antaranya bisa dijumlahkan untuk menghasilkan nilai target tertentu? Inilah inti dari **Subset Sum Problem (SSP)**!

**Subset Sum Problem** adalah masalah klasik dalam ilmu komputer yang termasuk dalam kategori **NP-Complete**. Diberikan sebuah himpunan bilangan bulat dan sebuah nilai target, tugas kita adalah menentukan apakah ada **subset** dari himpunan tersebut yang jumlah elemennya sama dengan nilai target. SSP tergolong dalam *decision problem*, yang berarti jawabannya cukup "ya" atau "tidak". Intinya adalah mencari apakah mungkin memilih sejumlah angka dari sekumpulan bilangan bulat sehingga totalnya sama persis dengan angka target.

---

## 🧠 Variasi Masalah Subset Sum Problem

Meskipun konsep dasarnya sama, SSP memiliki beberapa variasi:

* **Unbounded Subset Sum Problem**: Elemen dapat digunakan berkali-kali.
* **Multi-target Subset Sum Problem**: Mencari subset untuk beberapa nilai target sekaligus.
* **Approximate Subset Sum**: Mencari subset yang jumlahnya sedekat mungkin dengan target, bukan harus sama persis.
* **Partition Problem**: Sebuah kasus khusus SSP di mana targetnya adalah setengah dari total jumlah semua elemen, yaitu membagi himpunan menjadi dua subset dengan jumlah yang sama.

---

## 🧑‍💻 Pendekatan Solusi untuk Subset Sum Problem

Karena sifatnya yang NP-Complete, SSP tidak memiliki solusi polinomial yang efisien secara umum. Namun, ada beberapa pendekatan utama:

### 1. Pendekatan Rekursif (Brute Force)

* **Cara Kerja**: Mengecek semua kemungkinan subset secara eksplisit. Untuk setiap elemen dalam himpunan, ada dua pilihan: **sertakan** elemen tersebut dalam subset atau **abaikan**.
* **Basis Kasus**:
    * Jika `sum == 0`, berarti kita berhasil menemukan subset yang jumlahnya 0 (subset kosong), jadi `True`.
    * Jika `n == 0` (tidak ada elemen lagi) dan `sum != 0`, berarti kita tidak bisa mencapai target, jadi `False`.
* **Kompleksitas**: $O(2^n)$, karena ada $2^n$ kemungkinan subset yang harus diperiksa. Pendekatan ini tidak efisien untuk himpunan angka yang besar.

### 2. Pendekatan Pemrograman Dinamis (Dynamic Programming - DP)

DP digunakan untuk mengoptimalkan solusi rekursif dengan menghindari perhitungan ulang submasalah.

#### a. Memoization (Top-Down DP)

* **Cara Kerja**: Menggabungkan rekursi dengan penyimpanan hasil dari submasalah yang sudah dihitung (`cache`).
* **Struktur Data**: Gunakan tabel `dp[n][sum]` untuk menyimpan hasil. `dp[i][j]` akan menyimpan `True` jika target `j` dapat dicapai dengan menggunakan `i` elemen pertama.
* **Efisiensi**: Mengurangi pemanggilan ulang fungsi yang sama. Tetap membutuhkan *stack rekursi*.
* **Kompleksitas Waktu**: $O(n \times \text{sum})$
* **Kompleksitas Memori**: $O(n \times \text{sum})$

#### b. Tabulation (Bottom-Up DP)

* **Cara Kerja**: Pendekatan iteratif yang membangun solusi dari kasus paling kecil hingga kasus target.
* **Struktur Data**: Buat tabel `dp[n+1][sum+1]`. `dp[i][j]` akan `True` jika subset dari `arr[0...i-1]` bisa menghasilkan jumlah `j`.
* **Inisialisasi**:
    * `dp[i][0] = true` untuk semua `i` (subset kosong selalu menghasilkan jumlah 0).
    * `dp[0][j] = false` untuk semua `j > 0` (tidak bisa mendapatkan jumlah positif tanpa elemen).
* **Iterasi**: Iterasi mengisi tabel dari bawah ke atas, berdasarkan keputusan apakah menyertakan elemen saat ini atau tidak.
* **Kompleksitas Waktu**: $O(n \times \text{sum})$
* **Kompleksitas Memori**: $O(n \times \text{sum})$, namun bisa dioptimalkan menjadi $O(\text{sum})$.

---

## 📥 Input & 📤 Output

### Input

Biasanya berupa:

* Sebuah array (atau `vector`) `A` berisi angka-angka bulat positif.
* Sebuah nilai target `S`, yang merupakan jumlah yang ingin dicapai dari kombinasi elemen-elemen dalam array.

### Output

Output bisa bervariasi tergantung implementasi, antara lain:

* **Boolean**: `true` (ada subset) atau `false` (tidak ada subset).
* **Daftar Subset**: Menampilkan subset-subset yang jumlahnya `S`.
* **Jumlah Subset**: Menghitung berapa banyak subset yang valid.

---

## 🧮 Langkah Penyelesaian (Menggunakan Konsep Backtracking/Rekursif)

Meskipun kode contoh menggunakan DP, mari kita bayangkan alur pikir rekursif yang mendasari SSP (serupa dengan N-Queens):

1.  **Pilih atau Lewati Elemen**: Untuk setiap elemen dalam himpunan, kita punya dua pilihan:
    * **Inklusi**: Sertakan elemen saat ini dalam subset dan lanjutkan dengan sisa `target - elemen_saat_ini`.
    * **Eksklusi**: Lewati elemen saat ini dan lanjutkan dengan `target` yang sama.
2.  **Batasan (Constraint Check)**:
    * Jika `target` menjadi negatif, berarti jalur ini tidak valid.
    * Jika semua elemen sudah diproses dan `target` belum 0, berarti tidak ditemukan solusi melalui jalur ini.
3.  **Rekursi (Recursive Exploration)**: Panggil fungsi secara rekursif untuk kedua pilihan (sertakan atau lewati).
4.  **Backtrack**: Jika satu jalur rekursif tidak menghasilkan solusi, "mundur" ke keputusan sebelumnya dan coba jalur lain.
5.  **Basis Kasus (Base Case)**:
    * Jika `target` mencapai 0, berarti subset ditemukan (`true`).
    * Jika semua elemen sudah diproses dan `target` belum 0, berarti tidak ada subset yang ditemukan (`false`).

---

## 🧩 Problem and Solution Example (Alur Pikir Rekursif)

**CARI SUBSET DARI [3, 4, 5] YANG JUMLAHNYA 9**

Mari kita telusuri alur pikir (mirip rekursif/backtracking):

* **Mulai dengan target 9 dan elemen [3, 4, 5]**

1.  **Pertimbangkan 3:**
    * **Pilih 3**: Sisa target = $9 - 3 = 6$. Elemen tersisa: `[4, 5]`.
        * **Pertimbangkan 4 (dengan target 6 dari sisa [4, 5]):**
            * **Pilih 4**: Sisa target = $6 - 4 = 2$. Elemen tersisa: `[5]`.
                * **Pertimbangkan 5 (dengan target 2 dari sisa [5]):**
                    * **Pilih 5**: Sisa target = $2 - 5 = -3$. ❌ (Lebih dari 9, tidak valid).
                    * **Lewati 5**: Sisa target = $2$. Elemen habis. ❌ (Tidak 0).
                * *Backtrack* ke keputusan 4.
            * **Lewati 4**: Sisa target = $6$. Elemen tersisa: `[5]`.
                * **Pertimbangkan 5 (dengan target 6 dari sisa [5]):**
                    * **Pilih 5**: Sisa target = $6 - 5 = 1$. Elemen habis. ❌ (Tidak 0).
                    * **Lewati 5**: Sisa target = $6$. Elemen habis. ❌ (Tidak 0).
                * *Backtrack* ke keputusan 3.
        * *Backtrack* ke keputusan awal.
    * **Lewati 3**: Sisa target = $9$. Elemen tersisa: `[4, 5]`.
        * **Pertimbangkan 4 (dengan target 9 dari sisa [4, 5]):**
            * **Pilih 4**: Sisa target = $9 - 4 = 5$. Elemen tersisa: `[5]`.
                * **Pertimbangkan 5 (dengan target 5 dari sisa [5]):**
                    * **Pilih 5**: Sisa target = $5 - 5 = 0$. ✅ (TARGET TERCAPAI!)
                        * **Solusi ditemukan**: `[4, 5]`
                    * *Backtrack* ke keputusan 5.
                * *Backtrack* ke keputusan 4.
            * **Lewati 4**: Sisa target = $9$. Elemen tersisa: `[5]`.
                * **Pertimbangkan 5 (dengan target 9 dari sisa [5]):**
                    * **Pilih 5**: Sisa target = $9 - 5 = 4$. Elemen habis. ❌ (Tidak 0).
                    * **Lewati 5**: Sisa target = $9$. Elemen habis. ❌ (Tidak 0).
                * *Backtrack* ke keputusan 4.
        * *Backtrack* ke keputusan awal.

**Solusi Akhir:** `[4, 5]` (Ini adalah salah satu solusi yang mungkin, tergantung urutan pencarian).

---

## 💻 Contoh Kode (C++ - Pendekatan Dynamic Programming)

```cpp
#include <iostream>
#include <vector>
#include <numeric> // Untuk std::accumulate (tidak dipakai di kode ini, tapi berguna)

using namespace std;

// Fungsi untuk memeriksa apakah ada subset yang jumlahnya sama dengan target
bool isSubsetSum(const vector<int>& nums, int target) {
    int n = nums.size();

    // Buat tabel DP (n+1 baris, target+1 kolom)
    // dp[i][s] akan true jika subset dari nums[0...i-1] dapat menghasilkan sum 's'
    vector<vector<bool>> dp(n + 1, vector<bool>(target + 1, false));

    // Basis Kasus 1: Subset dengan sum 0 selalu bisa dibuat (dengan subset kosong)
    // Untuk setiap jumlah elemen (dari 0 hingga n), sum 0 selalu bisa dicapai
    for (int i = 0; i <= n; i++) {
        dp[i][0] = true;
    }

    // Basis Kasus 2: Tanpa elemen (i=0), hanya sum 0 yang bisa dicapai.
    // Baris dp[0][s] (untuk s > 0) sudah false secara default dari inisialisasi, jadi tidak perlu diubah.

    // Isi tabel DP
    // i: merepresentasikan jumlah elemen yang dipertimbangkan dari array nums (hingga nums[i-1])
    // s: merepresentasikan target sum yang sedang dicoba dicapai
    for (int i = 1; i <= n; i++) { // Iterasi melalui setiap item (dari item pertama hingga ke-n)
        for (int s = 1; s <= target; s++) { // Iterasi melalui setiap kemungkinan sum (dari 1 hingga target)
            // Kasus 1: Jika elemen saat ini (nums[i-1]) lebih besar dari target sum 's'
            // Kita tidak bisa menyertakan elemen ini, jadi hasilnya sama dengan tidak menyertakannya
            if (s < nums[i - 1]) {
                dp[i][s] = dp[i - 1][s]; // Hasilnya sama dengan jika elemen ke-(i-1) diabaikan
            } else {
                // Kasus 2: Jika elemen saat ini (nums[i-1]) bisa disertakan
                // Kita punya dua pilihan:
                // a) Abaikan elemen saat ini (dp[i-1][s])
                // b) Sertakan elemen saat ini (dp[i-1][s - nums[i-1]])
                // Jika salah satu dari pilihan ini menghasilkan true, maka dp[i][s] adalah true
                dp[i][s] = dp[i - 1][s] || dp[i - 1][s - nums[i - 1]];
            }
        }
    }

    // Hasil akhir ada di dp[n][target], yang menunjukkan apakah sum 'target' bisa dicapai
    // dengan menggunakan 'n' elemen pertama (yaitu, seluruh array nums)
    return dp[n][target];
}

int main() {
    int n, target;
    cout << "Masukkan jumlah elemen dalam himpunan: ";
    cin >> n;

    if (n <= 0) {
        cout << "Jumlah elemen harus positif." << endl;
        return 1;
    }

    vector<int> nums(n);
    cout << "Masukkan elemen-elemen himpunan (pisahkan dengan spasi):\n";
    for (int i = 0; i < n; i++) {
        cin >> nums[i];
    }

    cout << "Masukkan nilai target sum: ";
    cin >> target;

    // Panggil fungsi untuk memeriksa subset sum
    if (isSubsetSum(nums, target)) {
        cout << "Terdapat subset dengan jumlah " << target << "." << endl;
    } else {
        cout << "Tidak ada subset yang memiliki jumlah " << target << "." << endl;
    }

    return 0;
}
```

---

## 📚 Analisis Kompleksitas

### Kompleksitas Waktu

* **Brute Force / Rekursif**: $O(2^n)$. Ini karena setiap elemen memiliki dua kemungkinan (disertakan atau tidak disertakan), menghasilkan $2^n$ kemungkinan subset.
* **Dynamic Programming (DP)**: $O(n \times \text{sum})$. Kita mengisi tabel DP berukuran $(n+1) \times (\text{target}+1)$, dan setiap sel diisi dalam waktu konstan. Ini jauh lebih efisien daripada brute force, terutama ketika `sum` tidak terlalu besar dibandingkan dengan $2^n$.

### Kompleksitas Ruang

* **Brute Force / Rekursif**: $O(n)$ untuk *stack rekursi* (kedalaman rekursi maksimal adalah $n$).
* **Dynamic Programming (DP)**: $O(n \times \text{sum})$ untuk tabel DP.
* **DP Dioptimalkan**: Jika kita hanya peduli pada apakah target sum dapat dicapai dan tidak perlu melacak subsetnya, tabel DP dapat dioptimalkan menjadi satu baris (atau dua baris) saja, mengurangi kompleksitas ruang menjadi $O(\text{sum})$.

---

## 🌟 Aplikasi di Dunia Nyata

Meskipun terlihat seperti masalah teoritis, Subset Sum Problem punya banyak aplikasi penting:

1.  **Kriptografi**: Digunakan dalam sistem kriptografi berbasis ransel (Knapsack Cryptosystem), di mana kesulitan menyelesaikan masalah Subset Sum menjadi dasar keamanan kunci publik.
2.  **Alokasi Sumber Daya**: Membantu memilih subset proyek atau item agar sesuai dengan anggaran, kapasitas, atau target tertentu, seperti dalam manajemen proyek atau perencanaan produksi.
3.  **Genetika dan Bioinformatika**: Digunakan untuk menganalisis kombinasi gen atau fragmen DNA dengan total ekspresi atau panjang tertentu.
4.  **Keuangan dan Investasi**: Membantu menentukan apakah target investasi dapat dicapai dengan memilih kombinasi aset yang tepat dalam portofolio.
5.  **Perancangan Permainan dan Teka-Teki**: Muncul dalam permainan atau teka-teki yang melibatkan pencarian kombinasi angka sesuai skor target, seperti dalam permainan teka-teki Sudoku (variasi) atau memecahkan kode.

---

## 💪 Kelebihan dan Kekurangan

### Kelebihan

* **Mengajarkan Teknik Algoritma Penting**: Merupakan contoh fundamental untuk memahami konsep *backtracking* dan *dynamic programming*.
* **Contoh Masalah NP-Complete**: Memberikan wawasan tentang kelas masalah yang sulit namun penting dalam ilmu komputer.
* **Dasar untuk Banyak Aplikasi Praktis**: Fondasi untuk berbagai masalah optimasi di dunia nyata.
* **Digunakan untuk Benchmarking**: Sering dipakai untuk menguji kinerja berbagai algoritma optimasi.

### Kekurangan

* **Kompleksitas Tinggi untuk Input Besar**: Untuk kasus terburuk (menggunakan pendekatan DP) di mana `target sum` sangat besar, kompleksitas $O(n \times \text{sum})$ bisa jadi tidak efisien. Untuk brute force, $O(2^n)$ bahkan lebih cepat tidak praktis.
* **Tidak Selalu Skalabel**: Tidak cocok untuk himpunan angka atau target yang sangat besar karena pertumbuhan eksponensial (brute force) atau pertumbuhan pseudo-polinomial (DP).
* **Bisa Terlalu Abstrak**: Bagi pemula, konsepnya mungkin terasa terlalu abstrak tanpa konteks aplikasi yang jelas.
* **Solusi Tidak Unik**: Seringkali ada lebih dari satu subset yang memenuhi target, dan masalah ini biasanya hanya meminta keberadaan, bukan keunikan.

---

## 🏁 Kesimpulan

**Subset Sum Problem (SSP)** adalah masalah penting dalam ilmu komputer yang bertujuan untuk menentukan keberadaan subset dari sekumpulan bilangan bulat yang jumlahnya sama dengan nilai target tertentu. Tergolong dalam kategori **NP-Complete**, SSP dapat diselesaikan dengan pendekatan **rekursif** (kurang efisien) atau **dynamic programming** (lebih optimal).

Meskipun kompleksitasnya bisa tinggi untuk input besar, Subset Sum memiliki aplikasi nyata yang luas, mulai dari **kriptografi, keuangan, pengelolaan sumber daya, hingga bioinformatika**. Ini menjadikannya masalah yang tidak hanya memiliki nilai teoritis tinggi sebagai sarana pembelajaran algoritma, tetapi juga relevan dan penting dalam berbagai domain praktis.

---