Nama: Ruasa Azizan Zihni
NIM: 235150301111046

1. **Model Matematika Motor DC (Persamaan Listrik dan Mekanik)**

**Penjelasan:**
Motor DC dapat dihitung menggunakan dua persamaan utama:
1. **Persamaan Listrik**:
   - Persamaan ini merupakan hubungan antara tegangan input (V), arus (i), dan kecepatan sudut (omega).
   - Di mana:
     - (V): Tegangan input.
     - (R): Resistansi armature.
     - (L): Induktansi armature.
     - (i): Arus armature.
     - (Ke): Konstanta GGL (Gaya Gerak Listrik).
     - (omega): Kecepatan sudut motor.
   - Hasil yang diberikan adalah:
     - Persamaan Listrik:
                  d
        V = Ke⋅ω + L⋅──(i(t)) + R⋅i(t)
                  dt

2. **Persamaan Mekanik**:
   - Persamaan ini menrupakan hubungan antara torsi yang dihasilkan motor, kecepatan sudut, dan momen inersia.
   - Di mana:
     - (Kt): Konstanta torsi motor.
     - (J): Momen inersia rotor.
     - (b): Koefisien gesekan viscous.
     - (omega): Kecepatan sudut motor.
   - Hasil yang diberikan adalah:
     - Persamaan Mekanik:
                  d
        Kt⋅i(t) = J⋅──(ω(t)) + b⋅ω(t)
                  dt
       
#### Kode:
Kode Python menggunakan library `sympy` untuk mendefinisikan simbol-simbol dan menulis persamaan listrik dan mekanik dalam bentuk simbolik.

```python
eq_electric = sp.Eq(V, R*i + L*sp.diff(i, t) + Ke*omega)
eq_mechanical = sp.Eq(Kt*i, J*sp.diff(omega, t) + b*omega)
```

### 2. **Transformasi Laplace pada Motor DC**

#### Penjelasan:
Transformasi Laplace digunakan untuk mengubah persamaan diferensial (dalam domain waktu) menjadi persamaan aljabar (dalam domain Laplace). Ini memudahkan analisis sistem, terutama untuk menghitung fungsi alih.

1. **Transformasi Laplace Persamaan Listrik**:
   - Persamaan listrik dalam domain waktu:
     \[
     V = R \cdot i + L \cdot \frac{di}{dt} + K_e \cdot \omega
     \]
   - Transformasi Laplace-nya:
     \[
     V(s) = (R + L \cdot s) \cdot I(s) + K_e \cdot \Omega(s)
     \]

2. **Transformasi Laplace Persamaan Mekanik**:
   - Persamaan mekanik dalam domain waktu:
     \[
     K_t \cdot i = J \cdot \frac{d\omega}{dt} + b \cdot \omega
     \]
   - Transformasi Laplace-nya:
     \[
     K_t \cdot I(s) = (J \cdot s + b) \cdot \Omega(s)
     \]

#### Kode:
Kode Python menggunakan `sp.laplace_transform` untuk mengubah persamaan listrik dan mekanik ke domain Laplace.

```python
eq_electric_laplace = sp.laplace_transform(eq_electric.lhs - eq_electric.rhs, t, s, noconds=True)
eq_mechanical_laplace = sp.laplace_transform(eq_mechanical.lhs - eq_mechanical.rhs, t, s, noconds=True)
```

#### Output:
- Transformasi Laplace persamaan listrik: \( V(s) = (R + L \cdot s) \cdot I(s) + K_e \cdot \Omega(s) \)
- Transformasi Laplace persamaan mekanik: \( K_t \cdot I(s) = (J \cdot s + b) \cdot \Omega(s) \)

---

### 3. **Fungsi Alih Hubungan antara Kecepatan Sudut (\(\omega\)) terhadap Tegangan (\(V\))**

#### Penjelasan:
Fungsi alih (\(\frac{\Omega(s)}{V(s)}\)) adalah rasio antara kecepatan sudut (\(\Omega(s)\)) dan tegangan input (\(V(s)\)) dalam domain Laplace. Fungsi alih ini menggambarkan respons sistem terhadap input tegangan.

1. **Langkah-langkah Menghitung Fungsi Alih**:
Dari persamaan mekanik Laplace:

K
t
⋅
I
(
s
)
=
(
J
⋅
s
+
b
)
⋅
Ω
(
s
)
K 
t
​
 ⋅I(s)=(J⋅s+b)⋅Ω(s)
Solusi untuk 
I
(
s
)
I(s):

I
(
s
)
=
(
J
⋅
s
+
b
)
K
t
⋅
Ω
(
s
)
I(s)= 
K 
t
​
 
(J⋅s+b)
​
 ⋅Ω(s)
Substitusi 
I
(
s
)
I(s) ke persamaan listrik Laplace:

V
(
s
)
=
(
R
+
L
⋅
s
)
⋅
I
(
s
)
+
K
e
⋅
Ω
(
s
)
V(s)=(R+L⋅s)⋅I(s)+K 
e
​
 ⋅Ω(s)
Substitusi 
I
(
s
)
I(s):

V
(
s
)
=
(
R
+
L
⋅
s
)
⋅
(
(
J
⋅
s
+
b
)
K
t
⋅
Ω
(
s
)
)
+
K
e
⋅
Ω
(
s
)
V(s)=(R+L⋅s)⋅( 
K 
t
​
 
(J⋅s+b)
​
 ⋅Ω(s))+K 
e
​
 ⋅Ω(s)
Selesaikan untuk Ω(s) / V(s):
Ω(s) / V(s) = Kt / (J⋅s+b)(L⋅s+R)+Ke⋅Kt

Output: 
Ω(s) / V(s) = Kt / (J⋅s+b)(L⋅s+R)+Ke⋅Kt

#### Kode:
Kode Python menggunakan `sp.solve` untuk menyelesaikan persamaan Laplace dan menghitung fungsi alih.

```python
I_s = sp.solve(eq_mechanical_laplace, I)[0]
eq_transfer_function = sp.Eq(V, (R + L*s)*I_s + Ke*Omega)
transfer_function = sp.solve(eq_transfer_function, Omega)[0] / V
```

### Kesimpulan:
- **Model Matematika**: Menghasilkan persamaan listrik dan mekanik motor DC.
- **Transformasi Laplace**: Mengubah persamaan diferensial ke domain Laplace untuk memudahkan analisis.
- **Fungsi Alih**: Menghitung respons kecepatan sudut (\(\omega\)) terhadap tegangan input (\(V\)) dalam domain Laplace.
