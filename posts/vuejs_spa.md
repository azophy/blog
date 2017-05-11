Vue.js SPA!
===========

## Alhamdulillah...

Setelahs seharian ngoprek-ngoprek Vue-router dan components, ngulik-ngulik data-binding dan props parsing, Alhamdulillah blog ini udah jadi SPA, yang artinya link-link di atas udah jalan! Yeeeaaaahhhh!

Mohon doanya agar bisa segera gue bikin tutorialnya :D

Notes:
- Buat binding ke component nya vue-router, pakai ```v-bind props``` di ```<router-view>``` ( _not recommended, but it does the work :v_ )
- jangan lupa nilai 0 di javascript sama dengan false
- di vue, ngeset properti baru dari object mesti pake Vue.set()
- manfaatin ```<script type="text/x-template" id="view_post_template"></script>``` buat nyimpan template component di Vue
- ngedit url background css dari element di Vue mesti pakai ```v-bind:style```