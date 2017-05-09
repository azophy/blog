Belajar Vue.js : membuat blog pake markdown di Github
=====================================================

Oke, jadi tulisan ini gue dedikasikan selain untuk mengisi blog gue yang masih kosong, juga untuk memotivasi untuk lebih banyak ngoprek teknologi-teknologi baru yang gue ketinggalan banget sekarang-sekarang ini.

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


## Final Code
```html
<!DOCTYPE html>
<html>
<head>
    <title>Azophy's Blog</title>
    <script type="text/javascript" src="https://unpkg.com/marked@0.3.6"></script>
    <script type="text/javascript" src="https://unpkg.com/vue@2.3.2/dist/vue.js"></script>
</head>
<body>

<div id="container">
<div class="header">
    <h1>Azophy's Blog</h1>
    <span>Just another blog</span>
</div>
<div class="content">
  <div class="post_box" v-for="post in posts">
    <div class="actual_post" v-html="post"></div>
    <hr/>
  </div>
</div>
<div class="footer">
    &copy; Abdurrahman Shofy Adianto 2017
</div>
</div>

</body>

<script type="text/javascript">
  var instance = new Vue({
  el: '#container',
  data: {
    post_files: [ 'posts/hello_world.md'],
    posts: [],
    config: null,
  },

  created : function() {
    var self = this;
    this.post_files.forEach(function(item, index) {
      self.fetchUrl(item, function(xhr) {
        self.posts.push(marked(xhr.responseText));
      });
    })
  },

  computed: {
  },

  methods: {
    // A simple AJAX function to load files
    fetchUrl: function (url, callback) {
      var xhr = new XMLHttpRequest()
      var self = this
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

