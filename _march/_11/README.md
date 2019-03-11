# #1 - Pentingnya _Arrow Function_ #
(11 Maret 2019)

_Arrow Function_ memiliki beberapa kelebihan, yaitu:

1. Lebih ringkas
1. Konteks `this` lebih jelas dan mudah dipahami (opini)

Berikut adalah contoh membuat fungsi:

``` javascript
// fn declaration
function add (x,y) {
  return x + y;
}

// fn expression
var add = function (x,y) {
  return x + y;
}

// Dengan menggunakan arrow function:

add = (x,y) => {
  return x + y;
}

```
<hr />

_Catatan: Perbedaan utama dari fn_declaration dan fn_expression adalah fn_declaration dapat dipanggil bahkan sebelum barisnya dipanggil, tetapi tidak dengan fn_expression_

``` javascript
// fn declaration
add(3,4) // returns 7
function add (x,y) {
  return x + y;
}

add(3,3); // returns 6

/*
==============================
*/

// fn expression
add(3,4) // returns Uncaught TypeError: add is not a function
var add = function (x,y) {
  return x + y;
}
// setelah fn expression dieksekusi
add(3,5) // returns 8
```

<hr />

_Arrow function_ juga dapat diterapkan ke _method_ Javascript, contohnya pada _method_ `map` :

```javascript
// regular function
users.map(function (user) {
  // operasi terhadap user
})

// arrow function
users.map(() => {
  // operasi terhadap user
})
```

_Arrow function_ juga menerapkan `implicit return`, artinya jika kita hanya memiliki satu baris di dalam fungsi tersebut, kita dapat mengabaikan `return statement`. Selain itu, jika input fungsinya hanya satu, dapat dituliskan tanpa `()`.

Sebagai contoh, misal kita mempunyai fungsi untuk mengambil _tweets_ menggunakan `uid` dengan kriteria jumlah favorit lebih dari 50 dan jumlah retweet lebih dari 50:

``` javascript
function getTweets (uid) {
  return fetch('https://api.users.com' + uid)
    .then(function (response) {
      return response.json()
    })
    .then(function (response) {
      return response.data
    })
    .then(function (tweets) {
      // jumlah favorite > 50
      return tweets.filter(function (tweet) {
        return tweet.stars > 50
      })
    })
    .then(function (tweets) {
      // jumlah retweet > 50
      return tweets.filter(function (tweet) {
        return tweet.rts > 50
      })
    })
}
```
Dapat diringkas menjadi:
```javascript
function getTweets (uid) {
  return fetch('https://api.users.com' + uid)
    .then(response => response.json())
    .then(response => response.data)
    .then(tweets => tweets.filter((tweet) => tweet.stars > 50))
    .then(tweets => tweets.filter((tweet) => tweet.rts > 50))
}
```

Selain itu, _arrow function_ juga memudahkan penggunaan konteks `this`. Sebagai contoh:
```javascript
  api.fetchPopularRepos(lang)
    .then(function (repos) {
      console.log("this", this)
    })
```

Pada baris kode tersebut, `this` tidak dapat diidentifikasi atau _undefines_. Hal ini dikarenakan konteks `this` setiap kali kita membuat fungsi baru akan berbeda dengan konteks `this` yang ada pada luar fungsi tersebut. Misal, seharusnya `this` menunjukkan `class` di mana fungsi tersebut berada, tetapi pada akhirnya `this` malah mengacu pada fungsi tersebut atau tidak terdefinisikan; konteks `this` pada fungsi berbeda dengan konteks `this` yang ada di luar fungsi tersebut.

Kita bisa mengakali hal ini dengan menambahkan `.bind(this)` untuk menyatakan kita mengacu pada `this` di luar `function` tersebut, contohnya:

```javascript
  api.fetchPopularRepos(lang)
    .then(function (repos) {
      this.setState(function () {
        return {
          repos: repos
        }
      });
    }.bind(this));
```

Namun, jika kita menggunakan _arrow function_ , konteks `this` dalam sebuah fungsi tidak berubah (fungsi tidak membuat konteks baru untuk `this`). Dapat dikatakan:

```javascript
// Baris ini:
  api.fetchPopularRepos(lang)
    .then(function (repos) {
      this.setState(function () {
        return {
          repos: repos
        }
      });
    }.bind(this));

// Setara dengan baris ini
  api.fetchPopularRepos(lang)
    .then((repos) => {
      this.setTate( () => {
        return{
          repos: repos
        }
      })
    })

// atau
  api.fetchPopularRepos(lang)
    .then( (repos) => {
      this.setState( () => console.log('repos', repos) || ({repos: repos}))
    })
```
Baris terakhir juga membantu untuk melakukan `console.log()` sekaligus melakukan `setState()`


### Referensi: https://tylermcginnis.com/arrow-functions/