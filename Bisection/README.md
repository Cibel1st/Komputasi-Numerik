## Algoritma Metode Bisection (Persamaan Non-Linear)

Metode Bisection (Bagi Dua) adalah metode numerik tertutup (*bracketing method*) untuk mencari akar persamaan non-linear $f(x) = 0$ berdasarkan **Teorema Nilai Antara**, dengan membagi dua interval $[a, b]$ secara berulang hingga konvergen menuju nilai akar hampiran.

### Spesifikasi Input & Output
* **Input:**
  * Fungsi non-linear kontinu $f(x)$
  * Batas bawah interval awal $a$ dan batas atas interval awal $b$ (dengan $a < b$)
  * Toleransi galat (*error tolerance*) $\epsilon$ (misal $10^{-6}$)
  * Batas maksimum iterasi `max_iter`
* **Output:** Nilai akar hampiran $c$ yang memenuhi $f(c) \approx 0$ beserta jumlah iterasi yang ditempuh.

### Langkah Algoritma

#### Tahap 1: Validasi Interval Awal (*Bracketing Check*)
1. Evaluasi nilai fungsi pada kedua ujung interval, yaitu $f(a)$ dan $f(b)$.
2. Periksa syarat pengurungan akar $f(a) \cdot f(b) < 0$. Jika tidak terpenuhi ($f(a) \cdot f(b) \ge 0$), hentikan algoritma karena interval $[a, b]$ tidak menjamin keberadaan akar tunggal di dalamnya.

#### Tahap 2: Pembagian Interval (*Bisection*) & Uji Konvergensi
Inisialisasi `iterasi = 0`, lalu lakukan perulangan selama `iterasi < max_iter`:
1. **Hitung Titik Tengah:** Bagi dua interval saat ini untuk memperoleh kandidat akar baru:

$$c = \frac{a + b}{2}$$

2. **Evaluasi Nilai Fungsi:** Hitung nilai $f(c)$.
3. **Pengecekan Kriteria Berhenti (*Stopping Criteria*):** Hentikan loop dan kembalikan $c$ jika salah satu kondisi berikut terpenuhi:
   * Nilai fungsi mendekati nol:

$$\vert{}f(c)\vert{} < \epsilon$$

   * Lebar setengah interval lebih kecil dari toleransi galat:

$$\frac{b - a}{2} < \epsilon$$

4. **Pembaruan Batas Interval (*Interval Update*):**
   * Jika $f(a) \cdot f(c) < 0$, akar berada pada **sub-interval kiri** $[a, c]$. Perbarui batas atas dengan $b = c$.
   * Jika tidak ($f(c) \cdot f(b) < 0$), akar berada pada **sub-interval kanan** $[c, b]$. Perbarui batas bawah dengan $a = c$.
5. **Inkremen Iterasi:** Tambahkan nilai `iterasi = iterasi + 1`.

### Pseudocode

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

### Kompleksitas & Laju Konvergensi
* **Laju Konvergensi:** Linear dengan faktor reduksi galat sebesar $0.5$ di setiap iterasi. Jumlah minimum iterasi $n$ yang diperlukan untuk mencapai toleransi $\epsilon$ memenuhi persamaan:

$$
n \ge \left\lceil \log_2 \left( \frac{b - a}{\epsilon} \right) \right\rceil
$$

* **Kompleksitas Waktu:** $O\left(\log_2 \frac{b - a}{\epsilon}\right)$ evaluasi fungsi.
* **Kompleksitas Ruang:** $O(1)$.
