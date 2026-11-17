### 1. Spesifikasi Input dan Output

* **Input:** Matriks *augmented* $A$ berukuran $3 \times 4$ yang merepresentasikan bentuk $Ax = b$:

$$A = \begin{bmatrix} a_{00} & a_{01} & a_{02} & \big\vert{} & b_0 \\ a_{10} & a_{11} & a_{12} & \big\vert{} & b_1 \\ a_{20} & a_{21} & a_{22} & \big\vert{} & b_2 \end{bmatrix}$$


* **Output:** Nilai solusi variabel $x_1, x_2, x_3$ (atau pesan kesalahan jika sistem tidak memiliki solusi tunggal/singular).

---

### 2. Deskripsi Langkah Algoritma

#### Tahap 1: Eliminasi Maju (*Forward Elimination*) & *Partial Pivoting*

Tahap ini bertujuan mengubah matriks *augmented* menjadi **matriks segitiga atas** (*upper triangular matrix*) dengan mengeliminasi elemen di bawah diagonal utama secara bertahap untuk kolom pivot $k = 0$ dan $k = 1$:

1. **Pencarian Elemen Pivot Maksimum (*Partial Pivoting*):**
Pada kolom $k$, cari baris $r$ (dengan $r \ge k$) yang memiliki nilai mutlak elemen $\vert{}A[r][k]\vert{}$ terbesar. Langkah ini mencegah pembagian dengan nol (*division by zero*) serta meminimalkan galat pembulatan (*round-off error*).
2. **Pengecekan Singularitas:**
Jika nilai $\vert{}A[r][k]\vert{} \approx 0$ (lebih kecil dari toleransi $\epsilon$, misal $10^{-9}$), maka matriks bersifat **singular** (sistem tidak memiliki solusi unik). Hentikan proses.
3. **Pertukaran Baris (*Row Swapping*):**
Jika $r \neq k$, tukar seluruh elemen pada baris $k$ dengan baris $r$.
4. **Operasi Baris Elementer (OBE):**
Untuk setiap baris $i$ di bawah baris pivot ($i = k + 1$ hingga $2$):
* Hitung faktor pengali:

$$\text{faktor} = \frac{A[i][k]}{A[k][k]}$$


* Perbarui setiap elemen pada baris $i$ untuk kolom $j = k$ hingga $3$:

$$A[i][j] = A[i][j] - (\text{faktor} \times A[k][j])$$





#### Tahap 2: Substitusi Balik (*Back Substitution*)

Setelah terbentuk matriks segitiga atas, periksa terlebih dahulu apakah elemen pivot terakhir $A[2][2] \approx 0$. Jika tidak nol, hitung nilai variabel dari baris terbawah ke atas:

1. Hitung $x_3$ dari baris ke-2:

$$x_3 = \frac{A[2][3]}{A[2][2]}$$


2. Hitung $x_2$ dari baris ke-1 menggunakan nilai $x_3$:

$$x_2 = \frac{A[1][3] - (A[1][2] \times x_3)}{A[1][1]}$$


3. Hitung $x_1$ dari baris ke-0 menggunakan nilai $x_2$ dan $x_3$:

$$x_1 = \frac{A[0][3] - (A[0][1] \times x_2) - (A[0][2] \times x_3)}{A[0][0]}$$



---

### 3. Pseudocode Terstruktur

```text
ALGORITMA EliminasiGauss_SPLTV(A[3][4])
DEKLARASI:
    k, i, j, r : integer
    faktor, temp, eps : real
    x1, x2, x3 : real

DESKRIPSI:
    eps <- 1e-9

    // 1. Tahap Eliminasi Maju
    FOR k <- 0 TO 1 DO
        // a. Partial Pivoting: cari baris r >= k dengan |A[r][k]| terbesar
        r <- k
        FOR i <- k + 1 TO 2 DO
            IF abs(A[i][k]) > abs(A[r][k]) THEN
                r <- i
            ENDIF
        ENDFOR

        // b. Cek matriks singular
        IF abs(A[r][k]) < eps THEN
            PRINT "Sistem singular (tidak ada solusi unik)"
            STOP
        ENDIF

        // c. Tukar baris k dengan baris r jika berbeda
        IF r != k THEN
            FOR j <- k TO 3 DO
                temp <- A[k][j]
                A[k][j] <- A[r][j]
                A[r][j] <- temp
            ENDFOR
        ENDIF

        // d. Eliminasi elemen di bawah pivot
        FOR i <- k + 1 TO 2 DO
            faktor <- A[i][k] / A[k][k]
            FOR j <- k TO 3 DO
                A[i][j] <- A[i][j] - (faktor * A[k][j])
            ENDFOR
        ENDFOR
    ENDFOR

    // Cek pivot terakhir sebelum substitusi balik
    IF abs(A[2][2]) < eps THEN
        PRINT "Sistem singular (tidak ada solusi unik)"
        STOP
    ENDIF

    // 2. Tahap Substitusi Balik
    x3 <- A[2][3] / A[2][2]
    x2 <- (A[1][3] - A[1][2] * x3) / A[1][1]
    x1 <- (A[0][3] - A[0][1] * x2 - A[0][2] * x3) / A[0][0]

    // 3. Kembalikan hasil
    RETURN x1, x2, x3
END ALGORITMA

```

---

### Ringkasan Kompleksitas

* **Kompleksitas Waktu:** Untuk kasus umum $n$ variabel adalah $O(n^3)$, sedangkan untuk kasus tetap $n = 3$ jumlah operasinya bersifat konstan $O(1)$ (hanya melakukan 2 iterasi pivot utama dan 3 operasi substitusi balik).
* **Kompleksitas Ruang:** $O(1)$ secara *in-place* karena modifikasi dilakukan langsung pada matriks $A[3][4]$.
