## Pengenalan Asynchronous

**Javascript** adalah bahasa pemrograman *single-thread* yang artinya hanya dapat mengeksekusi satu *task* pada satu waktu atau biasa disebut ***Synchronous***.

Salah satu konsep lain di pemrograman adalah kebalikan dari *Synchronous* yaitu ***Asynchronous***.

Apa itu *Asynchronous*?
*Asynchronous* mengizinkan komputer kita untuk memproses *task* lain sambil menunggu suatu proses lain yang sedang berlangsung. Artinya kita bisa melakukan 2 proses sekaligus.

Contoh analoginya seperti ini:

Saat kita mencuci baju di mesin cuci. Agar lebih produktif, sambil menunggu cucian selesai kita bisa melakukan pekerjaan lain misalnya menyapu. Artinya disini kita melakukan 2 proses sekaligus.

Jika *javascript* adalah bahasa pemrograman *single-thread* lalu bagaimana kita membuat menjadi *asynchronous*?

Untuk bisa menggunakan *asynchronous* pada *javascript* kita dapat melakukan secara simulasi atau tidak murni yaitu menggunakan beberapa cara diantaranya:

1. ***Callback***
2. ***Promises***
3. ***Async / Await***

## Callback
Apa itu ***Callback***?

*Callback* adalah sebuah function. Namun bedanya dengan *function* pada umumnya adalah pada cara eksekusinya.

Jika *function* pada umumnya di eksekusi secara langsung sedangkan *callback* di eksekusi dalam *function* lain melalui parameter.

Kita akan menemukan proses *callback asynchronous* pada proses Ajax, komunikasi HTTP, Operasi file, timer, dan sebagainya.

Pada *synchronous* output di proses berdasarkan urutan kode.

## Contoh proses *synchronous*

```js
function proses1() {
  console.log("proses 1 selesai dijalankan");
}

function proses2() {
  console.log("proses 2 selesai dijalankan");
}

function proses3() {
  console.log("proses 3 selesai dijalankan");
}

proses1();
proses2();
proses3();

/*
Hasil Output
proses1 selesai dijalankan
proses2 selesai dijalankan
proses3 selesai dijalankan
*/
```

Pda kode diatas, kita bisa melihat bahwa proses1, proses2, dan proses3 berjalan berurutan seperti yang seharusnya.

## Contoh proses *asynchronous*

```js
function proses1() {
  console.log("proses 1 selesai dijalankan");
}

function proses2() {
  // setTimeout or delay for asynchronous simulation
  setTimeout(function () {
    console.log("proses 2 selesai dijalankan");
  }, 100);
}

function proses3() {
  console.log("proses 3 selesai dijalankan");
}

proses1();
proses2();
proses3();

/*
Hasil Output
proses1 selesai dijalankan
proses3 selesai dijalankan
proses2 selesai dijalankan
*/
```
Bisa kita bisa lihat bahwa proses3 selesai terlebih dahulu dibanding proses2. Hal ini dikarenakan proses2 melakukan `setTimeout()` yang merupakan proses *asynchronous* sehingga proses3 selesai terlebih dibanding proses2.

Kita akan coba memperbaiki *asynchronous* di atas dengan memastikan output p1, p2, dan p3 sesuai urutan dengan menggunakan *callback*.

```js
function p1() {
 console.log('proses 1 selesai dijalankan')
}

function p2(callback) {
 setTimeout(
  function() {
   console.log('proses 1 selesai dijalankan')
    callback()
  },100)
}

function p3() {
  console.log('proses 1 selesai dijalankan')
}
p1()
p2(p3)

/*
Hasil Output
proses1 selesai dijalankan
proses2 selesai dijalankan
proses3 selesai dijalankan
*/
```

Analogi kasus diatas adalah bayangkan kamu memiliki *method* yang melakukan *request* data API untuk menampilkan *image*. Kita memerlukan *callback* untuk memastikan data *image* terpanggil terlebih dahulu sebelum menampilkan ke user yang mengaksesnya. Jadi *callback* dapat digunakan untuk mengatur order *function* yang harus berjalan terlebih dahulu.

## Promise
***Promise*** adalah salah satu fitur dari *ES6* yang merupakan cara lain untuk melakukan *Asynchronous*.

*Promise* muncul untuk menyelesaikan masalah pada *callback*, yaitu callback hell.

## Contoh Callback Hell
*Callback Hell* adalah ketika ada *callback* didalam *callback* dan seterusnya. Problemnya adalah kode sulit dibaca dan penanganan *error-nya* juga menjadi sulit. Disaat seperti ini maka *promise* menjadi solusi.

Contoh penggunaan dari *Callback Hell*:

```js
const verifyUser = (username, password, callback) => {
  dataBase.verifyUser(username, password, (error, userInfo) => {
    if (error) {
      callback(erros);
    } else {
      dataBase.getRoles(username, (error, roles) => {
        if (error) {
          callback(error);
        } else {
          dataBase.logAccess(username, (error) => {
            if (error) {
              callback(error);
            } else {
              callback(null, userInfo, roles);
            }
          });
        }
      });
    }
  });
};
```

## 3 Status Promise

Analaogi dari sebuah promise itu sama seperti kita saat mengambil suatu data baik itu dari database maupun API. Akan ada 3 kondisi yaitu data berhasil didapatkan, data sedang diproses, dan data gagal didapatkan.

Pada promise analogi diatas bisa diartikan seperti:

1. 'fulfilled': Jika data telah berhasil didapatkan
2. 'pending': Jika data sedang diproses
3. 'rejected': Jika data gagal didapatkan

## Contoh pembuatan promise

<!-- ![async-promise1](../asynchronus/assets/async-promise1.png) -->

```js
let newPromise = new Promise((resolve, reject) => {
  if (true) {
    // apa yang dilakukan jika promise fulfilled
    resolve("Berhasil");
  } else {
    // apa yang dilakukan jika promise rejected
    rejected("Gagal");
  }
});
```

Kita bisa membuat sendiri apa yang akan dilakukan pada sebuah promise. Didalam promise ada 2 *keyword* yaitu `resolve()` dan `reject()`.

- `resolve()` jika proses berhasil atau *fullfilled* .
- `reject()` jika proses gagal atau *rejected*.

## Contoh penggunaan promise fullfilled

Untuk *fulfilled* hanya bisa tereksekusi jika kita kondisi berhasil pada saat kita melakukan async. Kita set condition menjadi `true` untuk simulasi *fulfilled*.

```js
const condition = true;

let newPromise = new Promise((resolve, reject) => {
  if (condition) {
    // apa yang dilakukan jika promise 'fulfilled'
    console.log('Berhasil')
    resolve("Berhasil");
  } else {
    // apa yang dilakukan jika promise 'rejected'
    console.log('Gagal')
    reject(new Error("Error Gagal"));
  }
});
```

## Contoh penggunaan promise rejected

Untuk *rejected* hanya bisa tereksekusi jika kita mengalami *error* pada saat kita melakukan proses *asynchronous*. Kita set condition menjadi `false` untuk simulasi *rejected*.

```js
const condition = false;

let newPromise = new Promise((resolve, reject) => {
  if (condition) {
    // apa yang dilakukan jika promise 'fulfilled'
    console.log('Berhasil')
    resolve("Berhasil");
  } else {
    // apa yang dilakukan jika promise 'rejected'
    console.log('Gagal')
    reject(new Error("Error Gagal"));
  }
});
```

## Promise Instance

Pada banyak kasus biasanya kita dapat menggunakan *Promise Instance* seperti `.then()`, `.catch()`, dan `.finally()`. 

Contoh berikut penggunaan *Promise Instance* untuk proses mengambil data API dari [https://jsonplaceholder.typicode.com/](url):

```js
fetch('https://jsonplaceholder.typicode.com/todos/1')
  .then(response => response.json()) // Jika data berhasil didapatkan
  .then(json => console.log(json))
  .catch(error => console.log(error)) // Jika data tidak berhasil didapatkan
```

## Async Await
***Async/Await*** baru digunakan saat ada update ES8 dan dibangun menggunakan *Promise* jadi sebenarnya *Async/Await* dan *Promise* itu sama hanya berbeda dari sintaks dan cara penggunaannya saja.

Ada 2 kata kunci yang memiliki pengertian sebagai berikut:

- Async = mengubah *function* *Synchronous* menjadi *Asynchronous*
- Await = menunda eksekusi hingga proses *Asynchronous* selesai

## Async
Berikut ini contoh penggunaan dari *Async*:

```js
async function tesAsyncAwait() {
  return "Fulfilled";
}

const tesAsyncAwait = async () => {
  return "Fulfilled";
};
```

## Await

*Await* hanya bisa digunakan dalam *async function* dan *await* adalah keyword dalam *async* yang digunakan untuk menunda hingga proses *Asynchronous* selesai.

Beikut ini contoh penggunaan dari *Async/Await*:

```js
const myAsyncFunction = async condition => {
  if (condition) {
    return 'Condition is fulfilled'
  } else {
    throw 'Condition is rejected'
  }
}

const run = async condition => {
  try {
    const message = await myAsyncFunction(condition)
    console.log(message)
  } catch (error) {
    console.log(error)
  }
}

run(true); // Condition is fulfilled
run(false); // Condition is rejected
```

## Fetch
Dalam javascript kita bisa mengirimkan *network request* dan juga bisa mengambil informasi terbaru dari server jika dibutuhkan.

Contoh *network request* yang biasa kita lakukan:

- Mengirimkan *form* login
- Mengambil informasi data user
- Mendapatkan notifikasi

Ada berbagai macam cara untuk bisa melakukan *network request* salah satunya menggunakan *method* `fetch()`.

Proses melakukan `fetch()` adalah salah satu proses asynchronous di *Javascript* sehingga kita perlu menggunakan salah satu diantara *promise* atau *async/await*.

## Fetch dengan Promise

Berikut ini contoh *fetch* data menggunakan *Promise*:

```js
fetch("https://jsonplaceholder.typicode.com/posts")
  .then(function (response) {
    return response.json();
  })
  .then(function (post) {
    console.log(post);
  });
```

Kode diatas mengambil data end-point API [JSONPlaceholder](https://jsonplaceholder.typicode.com/). Beikut ini tampilan dari `console.log()` untuk data yang kita panggil:

<p style="text-align: center;">
	<img src="https://skilvul-prod.s3-ap-southeast-1.amazonaws.com/lesson/js-intermediate/fetch-promise.png" alt="Console Tab" style="width: 90%;box-shadow: 10px 13px 17px 0px rgba(0,0,0,0.14);-webkit-box-shadow: 10px 13px 17px 0px rgba(0,0,0,0.14);-moz-box-shadow: 10px 13px 17px 0px rgba(0,0,0,0.14);">
</p>

Gambar diatas adalah hasil dari *fetch* menggunakan *Promise* yang kita lakukan.

## Fetch dengan Async/Await?

Berikut contoh fetch data menggunakan *async/await*

```js
const tesFetchAsync = async () => {
  let response = await fetch("https://jsonplaceholder.typicode.com/posts");
  response = await response.json();
  console.log(response);
};
tesFetchAsync();
```

Kita masih mengambil dari sumber yang sama dengan *fetch* menggunakan async/await sehingga hasilnya pun masih sama persis seperti sebelumnya.

Dalam penggunaan di dunia kerja dan aplikasi berskala besar kita bebas menggunakan *Promise* ataupun *Async/Await* tetapi kita lihat jika menggunakan *Async/Await*, kode kita terlihat lebih *clean* dan mudah dibaca.