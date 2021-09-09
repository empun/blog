---
title: "Cara Menggunakan Typescript di NodeJS"
date: 2021-09-09T16:58:36+07:00
draft: false
author: 'Empun'
categories:
- Nodejs
- Javascript
description: Menggunakan Typescript di NodeJS
---

## Intro

Bagi yang berlatang belakang mepakai bahasa pemrograman seperti Java atau C# memasuki dunia Javascript mungkin memberi sedikit nuansa berbeda. Javascript adalah bahasa yang dynamic typing yang tidak memberikan fitur type cheking. Intinya dia gak peduli apakah isinya string atau number atau yang lain. Sekarang diisi string nanti diisi array, gak akan ada notif, erronya muncul pas di compile. Nah biar ada notif dan nggak salah tipe data pas masukin ke parameter, atau nge-return sebuah function, kita pakai Typescript.

Typescript ini hanya bermanfaat di masa development. Pada akhirnya nanti akan di compile (transpile) jadi Javascript.

 TL;DR baca [conclusion](#conclusion) 🔖

----

## Part 1

Pastikan telah menginstall node.js

```bash
node -v
```

Ketikan `npm init -y`. Ini akan men-generate file `package.json`. 

```bash
npm init -y
```

Kurang lebih isinya akan seperti ini.

```json
{
  "name": "tutorial-install-typescript",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

Kemudian install `typescript` secara global.

```bash
sudo npm install -g typescript
```

Check kalau sudah ke `install`

```bash
tsc --version
```

Buat file `tsconfig.json` dengan mengetikan `tsc --init`. File ini akan berisi konfigurasi gimana project kita mau di-compile.

```bash
tsc --init
```

Dalam file ini ada beberapa konfigurasi penting.

- `"target": "es6"` . Isi aja `es6`. Supaya kita bisa memakai fitur seperti `const`, `let`, `arrow function`.
- `"module": "commonjs"`.  Bisa diisi `commonjs`  atau `es6`. 
- `"rootDir": "./src"`. Dimana file-file typescript kita disimpan. 
- `"outDir": "./dist"`. Dimana hasil compile nya akan disimpan.
- `"strict": true`. Hasil compile nanti akan memakai`strict mode`.
- `"esModuleInterop": true`. Ini supaya module ES6 bisa di compile ke module common.js
- `"exclude":["./node_modules"]`. Diisi array directory mana saja yang gak akan ikut ke-compile.

Ini adalah isi dari `tsconfig.json` yang kita buat. Settingan default bisa dihapus atau uncomment seperti bawaanya.

```json
{
  "compilerOptions": {                        
    "target": "es6",                               
    "module": "commonjs",                           
    "outDir": "./dist",                             
    "rootDir": "./src",                             
    "strict": true,
    "esModuleInterop": true,                       
  },
  "exclude":[
    "./node_modules"
  ]
}
```

Sekarang kita buat directory `src`. Didalamnya kita buat file `index.ts`.

```bash
mkdir ./src && cd src && touch index.ts
```

 Isi dengan typescript tentunya. Contohnya seperti ini

```typescript
function sum(val1: number, val2: number){
  return val1 + val2;
}
console.log(sum(12, 7))
```

Seharusnya kita sekarang sudah mempunyai `folder structure` seperti ini. 

![folder-structure](/1-folder-structure.png)

Jalankan perintah `tsc`. Perintah ini akan meng-compile file `index.ts` dalam directory `src` ke dalam directory `dist`. Seperti konfigurasi yang telah kita buat sebelumnya. 

```bash
tsc
```

Hasilnya akan akan ada file baru di folder `dist`, bernama `index.js`. 

![folder-structure](/1-folder-structure-2.png)
Tentu saja isinya seperti file Javascript pada umumnya 🐱.
```javascript
"use strict";
function sum(val1, val2) {
    return val1 + val2;
}
console.log(sum(12, 7));
```

Tinggal di run aja file `index.js` nya

```bash                                                                                                                                     
node dist/index.js
19
```

---

## Part 2

Sampai sini kita sudah bisa install typescript, bikin konfigurasinya, lalu di `transpile` ke Javascript. Problemnya tiap kita ubah file dalam `src`, kita harus ngetikan command `tsc` lagi, biar di terjemahin ke Javascript. Baru setelah itu kita run file `.js`-nya.  

Supaya lebih mudah kita akan install typescript executor bernama `ts-node`. Package ini akan meng-eksekusi file typescript, tanpa perlu compile ke javascript. 

Sebelumnya kita sudah install typescript secara global. Khusus untuk install package ini, kita juga harus install typescript di local project kita juga.

```bash
npm install -D typescript 
```

Baru deh install `ts-node`. Install secara local pakai `-D`.

```bash
sudo npm install -D ts-node  
```

Tambahin script berikut di file `package.json` untuk ngejalanin `ts-node` nya.

```json
 "scripts": {
    "start": "ts-node ./src/index.ts"
  },
```

---

✏️   Note: Langkah ini optional. Kalau sebelumnya `ts-node` nya di install secara global pakai `sudo npm install -G ts-node`, sebenarnya kita bisa langsung jalanin `ts-node`nya di command line. Contoh :

```bash
ts-node ./src/index.ts
```

---

Sekarang harusnya file `package.json` kita isinya seperti ini.

```json
{
  "name": "tutorial-install-typescript",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "ts-node ./src/index.ts"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "ts-node": "^10.2.1"
    "typescript": "^4.4.2"
  },
  "dependencies": {
    "express": "^4.17.1"
  }
}
```

Sekarang ketika membuat perubahan di file `./src/index.ts`  kita tinggal jalanin `npm start`. 

```bash
npm start

19
```

---

## Part 3

Sampai sini kita sudah bisa menggunakan typescript. Selanjutnya kita akan coba memakai fitur `type checking` dari typescript. Pada contoh ini kita akan menggunakanya pada library `express.js`.

Pertama pastikan kita install `@types/node`. Kenapa install ini ?.  Packages dari Node.js ditulis dalam javascript (ingat typescript hanya digunakan dalam tahap development, bukan production). Supaya mendapatkan `type definition` untuk `node` kita install `@types` untuk `node`.

```bash
npm install -D @types/node
```

Nah, untuk menggunakan `type definition` dari library `express.js` kita juga perlu install `type definition` untuk `express.js`.

```bash
npm install express
npm install -D @types/express
```

Harusnya isi `package.json` bagian `devDependencies `nya sudah berubah seperti ini.

```json
"devDependencies": {
  "@types/express": "^4.17.13",
  "@types/node": "^16.9.0",
  "ts-node": "^10.2.1",
  "typescript": "^4.4.2"
}
```

---

✏️ Note : Tidak semua library mempunyai `type definition`. Untuk mengeceknya check aja di [npm.js](https://www.npmjs.com/). Contoh untuk library [express](https://www.npmjs.com/package/express) dia mempunyai `type definition`. Ditandai dengan icon **DT**

![express-dt](/1-express-dt.png)

Library yang tidak punya tanda **DT**, artinya dia tidak punya `type definition`. Dan biasanya ketika di import akan muncul compile error. Untuk me-handle-nya (me-ignore-nya), bisa check question ini di [stack overflow](https://stackoverflow.com/questions/47388560/typescript-what-to-do-if-module-doesnt-have-types). Cuma ubah `tsconfig.json` dikit. Dan jangan khawatir, kebanyakan library populer sudah menyertakan `type definition`.

---

Sekarang kita ubah file `./src/index.ts`. Coba fitur library express nya.

```typescript
import express, { Request, Response, Application } from 'express';

const app: Application = express();
const PORT = process.env.PORT || 3000;

app.get("/", (req: Request, res: Response): void => {
    res.send("Lah udah ke kirim aja 🌵");
});

app.listen(PORT, (): void => {
    console.log(`Listening on 👉 https://localhost:${PORT}`);
});
```

Terkesan agak ribet emang. Misal pada baris 3, kita harus sertakan tipe `Application` untuk variabel `app`. Pada baris 6 kita harus sertakan tipe `Response` untuk variabel `res`. 

Tapi ini akan sangat membantu begitu project kita bertambah besar, selain itu kita juga bisa lebih mengenal library yang ktia pakai dengan mengetahui tipe parameter atau tipe nilai kembalian dari suatu function.  👏 🔥. Kalau yang biasa ngoding Java atau C# malah ngerasa nyaman sih. 🌳

Sekarang coba kita run. `npm start`

```bash                                                                                                                                        
npm start

> tutorial-install-typescript@1.0.0 start /Users/xxxx/Desktop/tutorial-install-typescript
> ts-node ./src/index.ts

Listening on 👉 https://localhost:3000
```

Contoh response :

![image-20210909103409583](/1-response-example.png)

Sebelumnya saya sudah hapus folder `./dist` nya. Karena kita sudah pakai `ts-node` tidak ada lagi folder `./dist` yang di generate ketika kita nge run `npm start`. Karena dia langsung nge-eksekusi file typescript-nya.

---

✏️ **Notes :** 
<br>
Supaya otomatis reload tanpa perlu jalanin `npm start`. Kita juga bisa install `nodemon` atau `ts-node-dev`. 

1️⃣ **Install ts-node-dev**

Ketikan perintah berikut.

```bash
npm install -D ts-node-dev
```
Edit bagian `scripts` di file `package.json`.
```json
"scripts": {
    "start": "ts-node ./src/index.ts",
    "dev1": "ts-node-dev --respawn ./src/index.ts"
}
```
Tinggal run scriptnya.
```bash
npm run dev1
```
 
2️⃣ **Install nodemon**

Ketikan perintah berikut.
```
npm install -D nodemon
```
Sama seperti sebelumnya. Disini saya kasih nama `dev2`.
```json
"scripts": {
    "start": "ts-node ./src/index.ts",
    "dev1": "ts-node-dev --respawn ./src/index.ts",
  	"dev2": "nodemon ./src/index.ts"
}
```
Jalanin `script` nya.
```bash
npm run dev2
```

Pilih salah satu. Keduanya sama, tinggal ubah bagian scripts di `package.json`.

---

## Conclusion

- Typescript membantu proses development. Kita di beritahu tipe data apa yang harus dimasukan sebagai parameter, apa yang harus di return, dan menampilkan error pada saat kompilasi. Membuat Javascript yang bersifat `dynamic typing` menjadi bahasa yang berasa `static typing` seperti Java dkk.

- Typescript akan di compile atau istilahnya disini di `transpile` jadi Javascript. Jadi yang di-running tetap kode Javascript.

**Quick Steps** 
- 1️⃣
- npm init -y 
- sudo npm install -g typescript
- tsc --init 👉 bakal ada tsconfig.json, setting file ini
- mkdir ./src && cd src && touch index.ts	👉 isi dengan typescript
- tsc	👉 bakal nge nge-compile jadi javascript	
- node dist/index.js	👉 jalanin javascript
- 2️⃣
- npm install -D typescript 	👉 install di local project, khusus buat jalanin ts-node
- npm install -D ts-node  	👉 package biar langsung eksekusi typescriptnya, tanpa nge compile
- 👉 setting dikit package.json nya
- npm start 	👉 jalanin ts-node sesuai settingan package.json nya
- 3️⃣
- npm install -D @types/node
- npm install express
- npm install -D @types/express
- 👉 buat kodingan express nya
- npm start

---

🌵 Written with ❤️ by : https://github.com/empun

---

