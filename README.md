# RasganeJS
Menggabungkan beberapa kode menjadi satu.

## Features
Tersedia ✔ || Belum Tersedia ❌

| Function | 1.0.0 | 1.0.1 |
| ------ | ------ | ------ |
| TimeAgo | ✔ | ❌ |
| Move Element | ✔ | ❌ |
| BloggerScript V.1.2.0 Custom | ✔ | ❌ |
| Klik Random Post | ✔ | ❌ |
| Up Button | ✔ | ❌ |
| Show Year | ✔ | ❌ |
| Tabs | ✔ | ❌ |
| Klik function | ✔ | ❌ |
| Custom Komen Disqus dan Blogger | ✔ | ❌ |

## Installation

Pasang kode RasganeJS di atas `</head>`.

Dapatkan file `RasganeJS.js`
Tambahkan kode dari file tersebut ke dalam tag `<script>`.

```
<script type='text/javascript'>/*<![CDATA[*/
  /*Add Code Here*/
/*]]>*/</script>
```

## How to use

### TimeAgo
digunakan untuk mengubah tanggal dan waktu yang ada dalam format ISO (seperti `2023-11-11T12:01:14+08:00`) menjadi format waktu relatif yang mudah dipahami, seperti "2 days ago" (2 hari yang lalu).

Fungsi ini akan mengubah semua elemen HTML dengan kelas .time yang berisi tanggal dan waktu menjadi format relatif yang lebih mudah dipahami.

### Tag HTML

```
<div class="time" datetime="2023-11-11T12:01:14+08:00">2023-11-11T12:01:14+08:00</div>
<div class="time">2013-11-12T12:01:14+08:00</div>
```

Tambahkan kode JavaScript untuk memanggil fungsi get_updateTimes setelah halaman dimuat untuk mengubah semua elemen dengan class `.time`.

```
<script type='text/javascript'>
  document.addEventListener('DOMContentLoaded', (event) => {
    get_updateTimes();
  });
</script>
```

Dalam Javascript (console.log)
```
console.log(get_TimeAgo('2023-06-01T12:00:00Z'));
```

### Move Element

#### Fungsi get_moveElement
Fungsi ini memindahkan sebuah elemen HTML dari satu tempat ke tempat lain, atau menggantinya dengan teks atau tag HTML jika elemen yang akan dipindahkan tidak ditemukan.

```
get_moveElement('Data_Elemen', 'Tujuan_Elemen', 'Text/Tag HTML');
```

##### Pejelasan
+ `Data_Elemen` : Selector dari elemen yang akan dipindahkan (misalnya, #data untuk elemen dengan id="data").
+ `Tujuan_Elemen` : Selector dari elemen tujuan (misalnya, #tujuan untuk elemen dengan id="tujuan").
+ `Text/Tag HTML` : Teks atau HTML yang akan ditempatkan di elemen tujuan jika elemen yang akan dipindahkan tidak ditemukan.

#### Fungsi get_moveElement2
Fungsi ini memindahkan sebuah elemen HTML dari satu tempat ke tempat lain, atau menghapus elemen tujuan jika elemen yang akan dipindahkan tidak ditemukan.

```
get_moveElement2('Data_Elemen', 'Tujuan_Elemen');
```
##### Pejelasan
+ `Data_Elemen` : Selector dari elemen yang akan dipindahkan (misalnya, #data untuk elemen dengan id="data").
+ `Tujuan_Elemen` : Selector dari elemen tujuan (misalnya, #tujuan untuk elemen dengan id="tujuan").

### BloggerScript V.1.2.0 Custom

### Klik Random Post
Fungsi ini menambahkan event listener ke elemen HTML yang dipilih untuk menampilkan postingan acak saat diklik.

```
get_random('ID/Class', 'Nama_Label');
```

```
get_random('ID/Class');
```

##### Pejelasan
+ `ID/Class` : Selector dari elemen HTML yang akan diklik (misalnya, #myButton untuk elemen dengan id="myButton").
+ `Nama_Label` : Label untuk memfilter postingan (opsional).

### Up Button
Fungsi ini menambahkan fungsionalitas ke elemen HTML yang berfungsi sebagai tombol "ke atas". Ketika tombol diklik, halaman akan menggulir ke atas secara halus. Selain itu, tombol dapat diberi kelas CSS tambahan saat pengguna menggulir ke bawah halaman.

```
get_up_button('ID/Class', 'Nama_Class');
```

```
get_up_button('ID/Class');
```

##### Pejelasan
+ `ID/Class` : Selector dari elemen HTML yang akan berfungsi sebagai tombol "ke atas" (misalnya, #upButton untuk elemen dengan id="upButton").
+ `Nama_Class` : Nama kelas CSS yang akan ditambahkan atau dihapus berdasarkan posisi scroll (opsional, misalnya active).

### Show Year
Fungsi ini mengambil tahun saat ini dan menampilkannya di elemen HTML yang dipilih berdasarkan selector yang diberikan.

```
get_year('ID/Class');
```

##### Pejelasan
+ `ID/Class` : Selector dari elemen HTML di mana tahun saat ini akan ditampilkan (misalnya, #year untuk elemen dengan id="year" atau .year untuk elemen dengan class="year".

### Tabs
Fungsi ini mengatur fungsionalitas tab navigasi di halaman web. Ketika sebuah tab diklik, tab tersebut menjadi aktif dan menampilkan konten yang sesuai, sementara tab dan konten lainnya disembunyikan.

```
<div class="box_tabs">
  <nav data-tabs=".box_tabs .content">
    <span class="tab_cp__ active" data-name="tab_1">Tab 1</span>
    <span class="tab_cp__" data-name="tab_2">Tab 2</span>
  </nav>
  <div class="content active" data-name="tab_1" style="display: block;">Tab 1</div>
  <div class="content" data-name="tab_2" style="display: none;">Tab 2</div>
</div>
```

```
get_tabs();
```

### Klik function
Fungsi ini digunakan untuk menambahkan atau menghapus kelas atau ID pada elemen target saat tombol diklik. Fungsi ini juga menghapus kelas atau ID dari elemen lain yang ditentukan agar hanya satu elemen yang memiliki kelas atau ID tersebut pada satu waktu.

```
get_klik('class yang ditambah', 'class/id button klik', 'class/id target');
```

```
get_klik2('class yang ditambah', 'class/id button klik', 'class/id target');
```

### Custom Komen Disqus dan Blogger
```
get_comment('disqus', 'click'); // Untuk memuat komentar Disqus dengan klik
```

```
get_comment('disqus', 'viewport'); // Untuk memuat komentar Disqus ketika elemen masuk viewport
```

```
get_comment('blogger'); // Untuk memuat komentar blogger
```