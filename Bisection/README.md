
---

### 1. Spesifikasi Input dan Output

* **Input:**
* Fungsi non-linear kontinu $f(x)$
* Batas bawah interval awal $a$ dan batas atas interval awal $b$ (dengan $a \lt b$)
* Toleransi galat (*error tolerance*) $\epsilon$ (misal $10^{-6}$)
* Batas maksimum iterasi $\text{max\_iter}$


* **Output:** Nilai akar hampiran $c$ yang memenuhi $f(c) \approx 0$ beserta jumlah iterasi yang ditempuh (atau pesan kesalahan jika interval awal tidak mengurung akar).

---

### 2. Deskripsi Langkah Algoritma

Metode Biseksi bekerja berdasarkan **Teorema Nilai Antara (*Intermediate Value Theorem*)**, yang menjamin bahwa jika fungsi kontinu $f(x)$ memiliki tanda yang berlawanan pada ujung-ujung interval $[a, b]$, maka terdapat minimal satu akar di dalam interval tersebut.

#### Tahap 1: Validasi Interval Awal (*Bracketing Check*)

1. Evaluasi nilai fungsi pada kedua batas interval, yakni $f(a)$ dan $f(b)$.
2. Periksa kondisi perubahan tanda dengan syarat $f(a) \cdot f(b) \lt 0$. Jika syarat tidak terpenuhi (bernilai positif atau nol), hentikan algoritma karena interval $[a, b]$ tidak menjamin keberadaan akar tunggal di dalamnya (kecuali jika $f(a) = 0$ atau $f(b) = 0$ yang berarti batas tersebut sudah merupakan akar eksak).

#### Tahap 2: Pembagian Interval (*Bisection*) & Uji Konvergensi

Inisialisasi penghitung $\text{iterasi} = 0$, kemudian lakukan perulangan selama $\text{iterasi} \lt \text{max\_iter}$:

1. **Hitung Titik Tengah:**
Bagi dua interval saat ini untuk mendapatkan kandidat akar baru $c$:

$$c = \frac{a + b}{2}$$


2. **Evaluasi Nilai Fungsi:**
Hitung nilai $f(c)$.
3. **Pengecekan Kriteria Berhenti (*Stopping Criteria*):**
Proses dihentikan dan $c$ ditetapkan sebagai akar hampiran apabila salah satu kondisi berikut terpenuhi:
* Nilai fungsi sudah cukup dekat dengan nol: $\vert{}f(c)\vert{} \lt \epsilon$
* Lebar setengah interval sudah lebih kecil dari toleransi galat: $\frac{b - a}{2} \lt \epsilon$


4. **Pembaruan Batas Interval (*Interval Update*):**
Tentukan sub-interval mana yang masih mengurung akar berdasarkan tanda perkalian fungsi:
* Jika $f(a) \cdot f(c) \lt 0$, akar berada di **sub-interval kiri** $[a, c]$. Perbarui batas atas dengan $b = c$.
* Jika tidak ($f(c) \cdot f(b) \lt 0$), akar berada di **sub-interval kanan** $[c, b]$. Perbarui batas bawah dengan $a = c$.


5. **Inkremen Iterasi:**
Tambahkan nilai $\text{iterasi} = \text{iterasi} + 1$.

---

### 3. Pseudocode Terstruktur

```text
ALGORITMA MetodeBisection(f, a, b, epsilon, max_iter)
DEKLARASI:
    a, b, c, fa, fb, fc, epsilon : real
    iterasi, max_iter : integer

DESKRIPSI:
    fa <- f(a)
    fb <- f(b)

    // 1. Cek syarat awal pengurungan akar (Teorema Nilai Antara)
    IF fa * fb >= 0 THEN
        PRINT "Interval awal [a, b] tidak mengurung akar (f(a) * f(b) >= 0)"
        STOP
    ENDIF

    // 2. Inisialisasi iterasi
    iterasi <- 0

    // 3. Perulangan pembagian interval
    WHILE iterasi < max_iter DO
        // a. Hitung titik tengah
        c <- (a + b) / 2.0
        fc <- f(c)

        // b. Cek konvergensi (nilai fungsi mendekati 0 atau lebar interval < epsilon)
        IF abs(fc) < epsilon OR (b - a) / 2.0 < epsilon THEN
            RETURN c, iterasi
        ENDIF

        // c. Tentukan sub-interval baru yang memuat akar
        IF fa * fc < 0 THEN
            b <- c
            fb <- fc
        ELSE
            a <- c
            fa <- fc
        ENDIF

        // d. Naikkan penghitung iterasi
        iterasi <- iterasi + 1
    ENDWHILE

    // 4. Kembalikan akar hampiran terbaik setelah mencapai batas iterasi
    RETURN c, iterasi
END ALGORITMA

```

---

### Ringkasan Kompleksitas & Konvergensi

* **Laju Konvergensi:** Konvergensi bersifat **linear** dengan faktor reduksi galat sebesar $0.5$ pada setiap langkah. Jumlah minimum iterasi $n$ yang dibutuhkan untuk mencapai toleransi $\epsilon$ dapat diprediksi secara analitis melalui:

$$n \ge \left\lceil \log_2 \left( \frac{b - a}{\epsilon} \right) \right\rceil$$


* **Kompleksitas Waktu & Ruang:** Kompleksitas waktu bergantung pada presisi yang diminta yaitu $O\left(\log_2 \frac{b-a}{\epsilon}\right)$ evaluasi fungsi, dengan kompleksitas ruang konstan $O(1)$.
