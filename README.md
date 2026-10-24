# Tugas Praktikum 8 - Pemrograman Berorientasi Objek

## Polymorphism, Abstract Class, Interface

<p align="center">
  <img width="286" height="286" alt="image" src="https://github.com/user-attachments/assets/c2271d06-376d-46e9-ac2f-8856b022f5ec" /><br>
  <b>Nama: Arya Lani Riyonggani Nurdian<br>
  NIM: L0325002</b>
</p>

## Penjelasan Kode

Seluruh kode berada di dalam `package` `com.praktikum8`. Praktikum ini terdiri dari tiga kegiatan, dan setiap kegiatan memiliki satu *main* untuk menjalankannya, yaitu `PoliTester`, `TesBangunDatar`, dan `TesSmartphone`.

## Kegiatan 1: Polymorphism (Method Overriding)

### Hewan.java

```java
package com.praktikum8;

public class Hewan {
    public void bersuara() {
        System.out.println("Hewan ini mengeluarkan suara yang tidak spesifik.");
    }
}
```

Class `Hewan` adalah *superclass* yang memiliki method `bersuara` dengan isi suara yang tidak spesifik. Method ini nantinya di-*override* oleh *subclass*.

### Kucing.java

```java
package com.praktikum8;

public class Kucing extends Hewan {
    @Override // Anotasi opsional tapi direkomendasikan
    public void bersuara() {
        System.out.println("Kucing: Meow meow!");
    }
}
```

### Anjing.java

```java
package com.praktikum8;

public class Anjing extends Hewan {
    @Override
    public void bersuara() {
        System.out.println("Anjing: Guk guk!");
    }
}
```

Class `Kucing` dan `Anjing` mewarisi `Hewan` menggunakan kata kunci `extends`. Keduanya me-*override* method `bersuara` sehingga masing-masing memiliki suara sendiri. Anotasi `@Override` membantu *compiler* memastikan method tersebut memang menimpa method milik *superclass*.

### PoliTester.java (main 1)

```java
package com.praktikum8;

public class PoliTester {
    public static void main(String[] args) {
        // Deklarasi array dari tipe Superclass, tetapi diisi dengan objek Subclass
        Hewan[] daftarHewan = new Hewan[3];
        daftarHewan[0] = new Hewan();
        daftarHewan[1] = new Kucing(); // Upcasting
        daftarHewan[2] = new Anjing(); // Upcasting

        // Polimorfisme: Pemanggilan method yang sama menghasilkan output berbeda
        for (Hewan h : daftarHewan) {
            h.bersuara();
        }
    }
}
```

Array `daftarHewan` bertipe `Hewan`, tetapi diisi dengan objek `Hewan`, `Kucing`, dan `Anjing`. Pengisian objek *subclass* ke variabel bertipe *superclass* disebut *upcasting*. Pada perulangan `for each`, method yang dipanggil selalu `bersuara`, tetapi isi yang dijalankan mengikuti jenis objek aslinya. Proses ini disebut *dynamic binding*, yaitu penentuan method yang dijalankan terjadi saat program berjalan (*runtime*).

### Output Program

```
Hewan ini mengeluarkan suara yang tidak spesifik.
Kucing: Meow meow!
Anjing: Guk guk!
```

### Penjelasan Output

Tiga baris output dihasilkan dari tiga elemen array secara berurutan. Elemen pertama adalah objek `Hewan`, sehingga yang tercetak adalah suara tidak spesifik. Elemen kedua adalah objek `Kucing`, sehingga tercetak suara kucing. Elemen ketiga adalah objek `Anjing`, sehingga tercetak suara anjing. Hasil ini menunjukkan bahwa satu pemanggilan `h.bersuara()` menghasilkan perilaku yang berbeda sesuai objeknya.

## Kegiatan 2: Abstract Class

### BangunDatar.java

```java
package com.praktikum8;

public abstract class BangunDatar {
    String warna;

    public BangunDatar(String warna) {
        this.warna = warna;
    }

    // Abstract method: Tidak memiliki isi, wajib di-override subclass
    public abstract double hitungLuas();

    // Concrete method: Punya isi, bisa langsung dipakai
    public void cetakWarna() {
        System.out.println("Warna bangun ini adalah: " + warna);
    }
}
```

`BangunDatar` dideklarasikan dengan kata kunci `abstract`, sehingga tidak bisa dibuat objeknya menggunakan `new`. Class ini memiliki satu *abstract method* yaitu `hitungLuas` yang tidak memiliki isi dan wajib di-*override* oleh *subclass*. Class ini juga memiliki *concrete method* `cetakWarna` yang sudah berisi dan bisa langsung dipakai oleh *subclass*.

### Lingkaran.java

```java
package com.praktikum8;

public class Lingkaran extends BangunDatar {
    private double jariJari;

    public Lingkaran(String warna, double jariJari) {
        super(warna);
        this.jariJari = jariJari;
    }

    @Override
    public double hitungLuas() {
        return Math.PI * jariJari * jariJari;
    }
}
```

Class `Lingkaran` mewarisi `BangunDatar`. *Constructor* memanggil `super(warna)` untuk mengisi atribut `warna` milik *superclass*, lalu mengisi `jariJari`. Method `hitungLuas` di-*override* dengan rumus luas lingkaran. Jika method ini tidak di-*override*, IDE akan menampilkan *error*.

### TesBangunDatar.java (main 2)

```java
package com.praktikum8;

public class TesBangunDatar {
    public static void main(String[] args) {
        Lingkaran lingkaran = new Lingkaran("Merah", 5);

        lingkaran.cetakWarna();
        System.out.println("Luas lingkaran: " + lingkaran.hitungLuas());
    }
}
```

Pada *main* ini dibuat objek `Lingkaran` berwarna "Merah" dengan jari-jari 5. Kemudian method `cetakWarna` dipanggil, dan luas lingkaran dihitung menggunakan `hitungLuas`.

### Output Program

```
Warna bangun ini adalah: Merah
Luas lingkaran: 78.53981633974483
```

### Penjelasan Output

Baris pertama dicetak oleh method `cetakWarna` milik `BangunDatar` yang diwariskan ke `Lingkaran`. Baris kedua berisi hasil `hitungLuas`, yaitu π × 5 × 5 yang bernilai 78.53981633974483.

## Kegiatan 3: Interface (Multiple Implementation)

### AlatElektronik.java

```java
package com.praktikum8;

public interface AlatElektronik {
    void nyalakan();
    void matikan();
}
```

### Kamera.java

```java
package com.praktikum8;

public interface Kamera {
    void ambilFoto();
}
```

Kedua *interface* hanya berisi tanda tangan method (*method signature*) tanpa isi. `AlatElektronik` memiliki method `nyalakan` dan `matikan`, sedangkan `Kamera` memiliki method `ambilFoto`.

### Smartphone.java

```java
package com.praktikum8;

public class Smartphone implements AlatElektronik, Kamera {
    private String merk;

    public Smartphone(String merk) {
        this.merk = merk;
    }

    @Override
    public void nyalakan() {
        System.out.println(merk + " menyala. Booting OS...");
    }

    @Override
    public void matikan() {
        System.out.println(merk + " dimatikan. Goodbye.");
    }

    @Override
    public void ambilFoto() {
        System.out.println("Cekrek! Foto berhasil disimpan di galeri " + merk + ".");
    }
}
```

Class `Smartphone` mengimplementasikan dua *interface* sekaligus menggunakan kata kunci `implements`. Dengan cara ini, Java dapat menyimulasikan pewarisan ganda (*multiple inheritance*). Ketiga method dari kedua *interface* wajib diisi oleh class ini.

### TesSmartphone.java (main 3)

```java
package com.praktikum8;

public class TesSmartphone {
    public static void main(String[] args) {
        Smartphone hp = new Smartphone("Samsung");

        hp.nyalakan();
        hp.ambilFoto();
        hp.matikan();
    }
}
```

Pada *main* ini dibuat objek `Smartphone` dengan merk "Samsung", lalu dipanggil tiga method secara berurutan yaitu `nyalakan`, `ambilFoto`, dan `matikan`.

### Output Program

```
Samsung menyala. Booting OS...
Cekrek! Foto berhasil disimpan di galeri Samsung.
Samsung dimatikan. Goodbye.
```

### Penjelasan Output

Tiga baris output sesuai urutan pemanggilan method pada *main*. Method `nyalakan` dan `matikan` berasal dari `AlatElektronik`, sedangkan `ambilFoto` berasal dari `Kamera`. Ketiganya dijalankan dari satu objek `Smartphone`, sehingga terbukti bahwa satu class dapat memenuhi lebih dari satu *interface*.
