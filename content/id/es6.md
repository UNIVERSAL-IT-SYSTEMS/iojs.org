# ES6 pada io.js

io.js dikembangkan dengan versi modern dari [V8](https://code.google.com/p/v8/). Dengan menjaga tetap teranyar dengan rilis terbaru dari *engine* ini kami memastikan fitur terbaru dari [JavaScript ECMA-262 spesifikasi](http://www.ecma-international.org/publications/standards/Ecma-262.htm)  tersedia untuk pengembang io.js dengan tepat waktu, serta perbaikan yang berkelanjutan pada kinerja dan stabilitas.

Versi {{project.current_version}} dari io.js dikeluarkan dengan V8 {{project.current_v8}}, yang meliputi ES6 fitur jauh melampaui versi 3.26.33 yang akan dikirim dengan joyent/node@0.12.x.

## Tidak ada lagi --harmony flag

Di joyent/node@0.12.x (V8 3.26), `--harmony` *flag* runtime memungkinkan semua **diselesaikan**, **staged** dan **in progress** fitur ES6 bersama-sama, dalam jumlah besar (dengan pengecualian tidak standar/semantik non-harmonis untuk `typeof` yang tersembunyi di bawah` --harmony-typeof`). Ini berarti bahwa beberapa fitur yang sangat *buggy* atau bahkan rusak seperti [proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy) hanya sebagai tersedia untuk pengembang sebagai [generators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*), yang memiliki sangat sedikit atau bahkan tidak ada yang diketahui-masalah. Dengan demikian, itu praktek terbaik baik memungkinkan hanya fitur tertentu dengan menggunakan flag fitur runtime harmoni tertentu (misalnya `--harmony-generators`), atau dengan mengaktifkan semua dari mereka dan kemudian menggunakan subset terbatas.

Dengan io.js (V8 4.1+), semua kerumitan itu hilang. Semua fitur harmoni sekarang dibagi dengan logis menjadi tiga kelompok untuk **shipping**, **staged** dan **in progress** fitur:

* Semua fitur **shipping**, fitur-fitur yang dianggap stabil oleh V8, seperti [classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes), [generators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*), [templates](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/template_strings), [new string methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_6_support_in_Mozilla#Additions_to_the_String_object) dan banyak lagi yang berubah **secara default pada io.js** dan **TIDAK** memerlukan *flag* runtime apapun.
* Lalu ada fitur **staged** dimana fitur yang belum sepenuhnya diuji atau belum diperbarui dengan spesifikasi terbaru dan oleh karena itu tidak dianggap stabil oleh tim V8 (misalnya mungkin ada beberapa kasus yang belum ditemukan). Ini mungkin sama dengan generator [generator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*) pada versi 3.26. Ini adalah "penggunaan di risiko Anda sendiri" jenis fitur yang sekarang memerlukan flag runtime: `--es_staging` (atau sinonimnya,` --harmony`).
* Akhirnya, semua fitur **in progress** masing-masing dapat dapat diaktifkan dengan *flag* harmoni masing-masing (misalnya `--harmony_arrow_functions`), meskipun hal ini sangat tidak disarankan kecuali untuk tujuan pengujian.

## Apa saja fitur ES6 yang ada pada dengan io.js (tanpa *flag* runtime yang diperlukan)?

* *Block scoping*

	* [let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)

	* [const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)

	* *`function`-in-blocks*

	>Sesuai dengan v8 3.31.74.1, deklarasi block-scoped [sengaja diimplementasikan dengan keterbatasan non-compliant pada strict mode](https://groups.google.com/forum/#!topic/v8-users/3UXNCkAU8Es). Para *developer* harus menyadari bahwa perubahan ini terus menerus sesuai dengan penyesuaian v8 dengan spesifikasi ES6 .

* [Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) (hanya dalam *strict mode*)

*   Collections

    *   [Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

    *   [WeakMap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakMap)

    *   [Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)

    *   [WeakSet](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WeakSet)

*   [Generators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*)

*   [Literal biner dan oktal](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Numeric_literals)

*   [Ekstensi-ekstensi literal Object](https://github.com/lukehoban/es6features#enhanced-object-literals) (metode-metode dan properti-properti dalam bentuk *shorthand*)

*   [Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

*   [Metode String baru](https://developer.mozilla.org/en-US/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_6_support_in_Mozilla#Additions_to_the_String_object)

*   [Symbols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)

*   [Template strings](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/template_strings)

Anda dapat melihat daftar yang lebih rinci, termasuk perbandingan dengan *engines* lainnya, pada halaman proyek [compat-table](https://kangax.github.io/compat-table/es6/).


## Apa saja fitur ES6 yang ada dalam flag --es_staging?

*   [`Symbol.toStringTag`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol) (*user-definable results* untuk `Object.prototype.toString`, dibalik flag `--harmony_tostring`)

## Fitur-fitur ES6 apa saja yang sedang dalam perkembangan?

Fitur-fitur baru ditambahkan ke dalam *engine* V8 secara konstan. Umumnya, diharapkan fitur-fitur tersebut untuk hadir dalam perilisan io.js kedepan, walaupun waktu pastinya tidak diketahui.

Anda dapat melihat fitur-fitur apa saja yang sedang dalam perkembangan di setiap perilisan io.js dengan meng-*grep* argumen `--v8-options`. Harap diketahui bahwa fitur-fitur V8 ini tidak lengkap dan ada kemungkinan masih rusak, jadi mohon ditanggung resikonya.

```sh
iojs --v8-options | grep "in progress"
```

## Saya telah setup infrastruktur saya untuk memanfaatkan flag --harmony. Apakah harus saya hapus?

Fungsi flag --harmony pada io.js saat ini adalah untuk mengaktifkan fitur **staged** saja. Selain itu, fungsi tersebut adalah sinonim dari --es_staging. Seperti disebutkan di atas, fitur-fitur tersebut belum dianggap stabil. Jika Anda ingin aman, terutama pada lingkungan produksi, pertimbangkan untuk menghapus flag runtime ini sampai fitur-fitur tersebut menjadi fitur bawaan pada V8 dan io.js. Jika Anda tetap mengaktifkan ini, Anda harus bersiap-siap untuk mengganti sumber kode anda jika ada pembaruan io.js dan perubahan pada V8 terlalu signifikan.


## Bagaimana cara menemukan versi V8 dengan versi tertentu dari io.js?

io.js menyediakan cara sederhana untuk membuat daftar semua dependensi dan versi masing-masing yang dikirimkan dengan biner tertentu melalui objek `process` global. Dalam kasus V8 *engine*, ketik kode dibawah ini di terminal Anda untuk menampilkan versinya:

```sh
iojs p process.versions.v8
```
