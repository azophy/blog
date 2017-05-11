Belajar Vue.js : membuat blog pake markdown di Github
=====================================================

## Because, why not?

Oke, jadi tulisan ini gue dedikasikan selain untuk mengisi blog gue yang masih kosong, juga untuk memotivasi untuk lebih banyak ngoprek teknologi-teknologi baru yang gue ketinggalan banget sekarang-sekarang ini.

## Pengantar

Sebelum memulai, gue mungkin cerita-cerita dikit soal isi tulisan ini nantinya.

Pertama, soal Vue.js. Kenapa Vue? Jadi kalau boleh jujur, dari 10 tahun-an pengalaman gue ngoprek web, gue memang ngerasa kalau akhir-akhir ini gue _overwhelmed_ banget sama perkembangan trend tekonologi di bidang web akhir-akhir ini. _To be more specific :_  Gue gak kuat. Sejak muncul Ruby on Rails, node.js, dilanjutkan ada React, Angular, React Native, Laravel, dan sekarang ada Vue.js, menurut gue terlalu banyak yang harus gue pelajari. Gue sendiri kalau buat proyek-proyek sendiri sudah sangat nyaman dengan _stack_ yang gue pake (backend pake PHP dengan Yii2 framework, front-end nya CSS, JavaScript, sama JQuery seperlunya).

Tapi sayangnya zaman sekarang kalau mau bikin aplikasi yang memang sesuai tuntutan pasar, dalam waktu yang gak bikin user keburu kabur, gak mungkin mungkin orang kerja sendiri. Dan kalau mau kerja bareng artinya kita harus siap mengalah memakai _stack_ teknologi yang umum dipakai orang. Dan salah satu yang banyak dipakai orang akhir-akhir ini adalah Vue.

Oke, cukup segitu _back-story_ gue tentang Vue. Kalau sampai detik ini lu masih bingung apa itu Vue, tinggal tekan Ctrl+T, ketika "Apa itu Vue.js", dan tekan Enter. :v

Kedua. Kenapa Blogging, pake Markdown, di Github lagi?

Jawabannya : karena itu keren.

itu aja

_Its just so dang cool dude. What could any nerd be more proud of when someone just confused when you literally has to code just to post a single article? You feel me?_ 

Oke, _enough with the jokes_ , sebenarnya at least ada 2 alasannya kenapa blogging pakai Github. Pertama, gratis. Kedua, freedom. Ngeblogging di Github dengan memanfaatkan fitur Github pages itu gratis, dan karena kita sendiri yang ngehosting berarti kita memiliki kebabasan yang sangat luas buat ngoprek-ngoprek website kita.

Kekurangannya tinggal satu: kita cuman bisa nge-upload halaman web statis ke Github. Jadi buat kalian-kalian yang kenal Geocities sama Tripod (hayoo, udah punya anak mestinya kalian kalau tahu 2 situs itu), itu persis kayak gitu. Artinya? Kita gak bisa bikin halaman dinamis kayak website-website yang berbasis Wordpress misalnya, dimana user bisa login dan ngepost komentar atau ngasih like di tulisan kita. Sebenarnya sih sebagian fitur-fitur itu bisa di-cover dengan memanfaatkan layanan-layanan pihak ketiga, kayak misalnya memakai widget Disqus atau Facebook Comment buat ngasih komentar, dst.

Yang paling kerasa itu sebenarnya soal nge-update link-link interna di blog kita setiap kali kita nambah konten. Misal kalau kita mau nambah satu halaman baru, dan di tiap halaman post kita ada link "10 post terakhir di blog ini", maka lu harus ngedit minimal 10 halaman web. Kebayang kan kalau harus dikerjain manual?

Karena itulah orang yang mau nge-blogging pake Github banyak yang memanfaatkan "Static Page Builder" macam Jekyll dkk.

Tapi memanfaatkan "Static Page Builder" berarti lu harus nginstall aplikasi baru. Buat gue, gue lebih prefer kalau gue gak perlu melakukan itu. Makanya muncul ide untuk membuat web berbasis Vue, dimana konten tulisan-tulisan gue di ambil mentah-mentah sebagai file markdown yang di convert jadi HTML sama client.

## Inisialisasi

Untuk file Javascript dari lirary yang kita butuhkan, di sini gue bakal memanfaatkan layanan Content Delivery Network gratisan dari unpkg.com

```html
    <script type="text/javascript" src="https://unpkg.com/marked@0.3.6"></script>
    <script type="text/javascript" src="https://unpkg.com/vue@2.3.2/dist/vue.js"></script>
```

HTML:
```html
<div id="container">
    <div class="header">
        <h1>My Blog</h1>
        <span>Just another blog</span>
    </div>
    <div class="content"></div>
    <div class="footer">
        &copy; Blogger 2017
    </div>
</div>
```

JavaScript:
```js
var instance = new Vue({
  el: '#container',

  data: {
    post_files: [],
    posts: [],
  },

  created : function() {},

  methods: {}
})

```

penjelasan:
- el : penunjuk ke nama element HTML yang akan menjadi wadah untuk semua urusan Vue kita
- data : untuk menyimpan semua variabel yang bakal perlu di-"main"-in sama Vue. Semua variabel yang kita definisikan di sini nantinya bisa dipakai buat templating di Vue nantinya.

Di sini gue cuman mendifinsikan 2 variabel: post_files buat menyimpan alamat dari semua file postingan gue, dan posts yang bakal menyimpan semua isi semua postingan tadi nantinya.
- created : ini file yang dipanggil ketika Vue.js dipanggil. Mirip-mirip $(document).ready() kalau di JQuery.
- methods : semua fungsi buatan sendiri yang dibutuhin di Vue bisa juga ditaruh di sini.

## Susunan file

Ini gambaran susunan proyek kita nantinya. Semua kode di atas nantinya di simpan di inde.html, sementara semua postingan yang ditulis pake markdown bisa di simpan di folder sendiri yang gue kasih nama 'posts'.

```
index.html
posts/
.. hello_world.md
```

Yang perlu di inget, di sini gue bikin postingan pake Markdown. Lu bisa aja ngoding HTML sendiri atau manfaatin syntax lain kayak RTF atau sekalian La-TeX. Lu otak-atik sendiri aja nantinya kode gue. Di sini gue ngasih contoh file markdown yang simple banget

```markdown
Judul Post
==========

Ini contoh isi post yang pake _italic_ dan **bold**.
```

## Membaca file

Untuk membaca file, di sini gue manfaatin fungsi AJAX "mentahan" dari JavaScript. Lu bisa juga manfaatin fungsi-fungsi luar (pakai $.ajax atau $.load nya JQuery misalnya), tapi di sini gue milih pake cara ini aja.
```js
  ...
  methods: {
    // A simple AJAX function to load files
    fetchUrl: function (url, callback) {
      var xhr = new XMLHttpRequest()
      xhr.open('GET', url)
      xhr.onload = function () {
        callback(xhr);
      }
      xhr.send()
    }
  }
  ...
```

Terus, kita bikin ubah fungsi 'created' kita buat membaca semua file yang kita list di variabel 'post_files' kita:

```js
  ...
  data: {
    post_files: [ 'posts/hello_world.md'],
    posts: [],
  },

  created : function() {
    var self = this;
    this.post_files.forEach(function(item, index) {
      self.fetchUrl(item, function(xhr) {
        self.posts.push(xhr.responseText);
      });
    })
  },
  ...
```

## Menampilkan isi file
Nah, di sini mulai keliatan kerennya Vue. Dengan Vue, kita bisa bikin "two-way data binding". Maksudnya, kita bisa nyambungin isi elemen-elemen HTML dengan variabel-variabel di Vue, sehingga ketika salah satu diubah, yang lainnya otomatis ngikut tanpa perlu kita ubah secara manual.

Contoh di sini adalah parameter 'v-for' yang intinya menelusurin isi dari suatu variabel (di sini contohnya variabel _posts_ ), dengan tiap elemen yang ditelusurin diwakilin sebagai variabel sebelum _keyword_ in (contoh di sini variabel _post_ gak pake 's'), terus isi dari tag yang dikasih 'v-for' tadi bakal diulang untuk setiap nilai _post_ .

Terus parameter 'v-html' bakal bikin isi dari kode html elemen yang bersangkutan ngikut dengan isi variabel yang bersangkutan.

Nah, jadi tiap kali kita mengubah isi variabel post, dari div di bawah bakal ngikut secara otomatis. Keren kan?

```html

    <div class="content">
      <div v-for="post in posts">
        <div class="actual_post" v-html="post"></div>
        <hr/>
      </div>
    </div>
```

## Konversi Markdown

Buat convert isi file kita, kita tinggal tambahin fungsi marked yang library-nya udah kita panggil sebelumnya:

```js
  ...
  created : function() {
    var self = this;
    this.post_files.forEach(function(item, index) {
      self.fetchUrl(item, function(xhr) {
        self.posts.push(marked(xhr.responseText));
      });
    })
  },
  ...
```

## Final Code

```html
<!DOCTYPE html>
<html>
<head>
    <title>My Blog</title>
    <script type="text/javascript" src="https://unpkg.com/marked@0.3.6"></script>
    <script type="text/javascript" src="https://unpkg.com/vue@2.3.2/dist/vue.js"></script>
</head>
<body>

<div id="container">
    <div class="header">
        <h1>My Blog</h1>
        <span>Just another blog</span>
    </div>
    <div class="content">
      <div v-for="post in posts">
        <div class="actual_post" v-html="post"></div>
        <hr/>
      </div>
    </div>
    <div class="footer">
        &copy; Blogger 2017
    </div>
</div>

</body>

<script type="text/javascript">
  var instance = new Vue({
  el: '#container',
  data: {
    post_files: [ 'posts/hello_world.md'],
    posts: [],
  },

  created : function() {
    var self = this;
    this.post_files.forEach(function(item, index) {
      self.fetchUrl(item, function(xhr) {
        self.posts.push(marked(xhr.responseText));
      });
    })
  },

  methods: {
    // A simple AJAX function to load files
    fetchUrl: function (url, callback) {
      var xhr = new XMLHttpRequest()
      xhr.open('GET', url)
      xhr.onload = function () {
        callback(xhr);
      }
      xhr.send()
    }
  }
})
</script>
</html>
```

## References:
- https://vuejs.org/v2/examples/commits.html : Contoh aplikasi Vue yang ngeload content dari tempat lain
- https://medium.freecodecamp.com/vue-js-introduction-for-people-who-know-just-enough-jquery-to-get-by-eab5aa193d77 : tutorial bagus banget

