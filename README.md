# **PyMongo, driver MongoDB resmi untuk Python.**

## **Mengenal PyMongo**

PyMongo adalah distribusi Python yang berisi alat untuk bekerja dengan MongoDB, MongoDB adalah sistem basis data berorentasi dokumen lintas platform. Diklasifikasikan sebagai basis data "NoSQL", MongoDB menghindari struktur basis data relasional tabel berbasis tradisional yang mendukung JSON seperti dokumen dengan skema dinamis (MongoDB menyebutnya sebagai format BSON), membuat integrasi data dalam beberapa jenis aplikasi lebih mudah dan lebih cepat. Python dapat digunakan dalam aplikasi database, salah satunya database NoSQL yang paling populer adalah MongoDB.

## **Menginstal PyMongo**

Menginstal PyMongo dapat di lakukan melalui perintah pip.

`pip install pymongo`

Melihat apakah instalasi berhasil atau tidak jika kita sudah menginstal `pymongo`, buatlah halaman Python dengan nama misalnya `mongodb.py`

```
import pymongo
```

Jika kode dijalankan tanpa kesalahan, artinya `pymongo` telah terinstal dan siap digunakan.

## **Memahami Cara Membuat Database pada MongoDB**

Membuat database di MongoDB, mulailah dengan membuat objek MongoClient, lalu tentukan URL koneksi dengan alamat ip yang benar dan nama database yang ingin kita buat.

- **Membuat Database**

Contohnya

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]
```

Kita membuat database dengan nama `perpustakaan`.

Di MongoDB, database tidak dibuat sampai mendapatkan konten. MongoDB menunggu hingga kita membuat koleksi (tabel), dengan setidaknya satu dokumen (catatan) sebelum benar-benar membuat database (dan koleksi).

- **Melihat Database pada MongoDB**

Kita akan melihat apakah database kita ada. Kita dapat melihat apakah ada database dengan mendaftar semua database di sistem yang telah kita buat.

Mengembalikan daftar database.

Contohnya

```
print(mongoclient.list_database_names())
```

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

print(mongoclient.list_database_names())
```

## **Memahami Cara Membuat Collection pada MongoDB**

Koleksi di MongoDB sama dengan tabel di database SQL. Membuat koleksi di MongoDB, gunakan objek database dan tentukan nama koleksi yang ingin kita buat.

- **Membuat Koleksi**

Contohnya

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]
```

Kita membuat koleksi dengan nama `buku`.

Di MongoDB, koleksi tidak dibuat sampai mendapatkan konten. MongoDB menunggu hingga kita memasukkan dokumen sebelum benar-benar membuat koleksi.

- **Melihat Koleksi**

Kita dapat memeriksa apakah ada koleksi dalam database dengan mendaftar semua koleksi.

Mengembalikan daftar semua koleksi di database.

Contohnya

```
print(db.list_collection_names())
```

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]

print(db.list_collection_names())
```

## **Memahami Cara Melakukan Insert pada MongoDB**

Dokumen di MongoDB sama dengan catatan di database SQL.

- **Masukkan Koleksi**

Menyisipkan catatan, atau dokumen seperti yang disebut di MongoDB, ke dalam koleksi, kita menggunakan insert_one() metode.

Parameter pertama dari insert_one()metode ini adalah kamus yang berisi nama dan nilai setiap field dalam dokumen yang ingin kita sisipkan.

Masukkan catatan dalam koleksi `buku`.

Contohnya
```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]

daftarbuku = { "namabuku": "MongoDB" }

notes = mongocol.insert_one(daftarbuku)

print(notes)
```

- **Mengembalikan _id**

Metode insert_one() mengembalikan objek InsertOneResult, yang memiliki properti, inserted_id, yang menyimpan id dari dokumen yang disisipkan.

Memasukkan catatan lain dalam koleksi "buku", dan Mengembalikan nilai _id.

Contohnya

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]

daftarbuku = { "namabuku": "MongoDB" }

notes = mongocol.insert_one(daftarbuku)

print(notes.inserted_id)
```

Jika kita tidak menentukan _id nya, maka MongoDB akan menambahkan satu untuk kita dan menetapkan id unik untuk setiap dokumen.

Dalam contoh kita tidak ada _id yang ditentukan, jadi MongoDB menetapkan _id unik untuk catatan (dokumen).

- **Memasukan Banyak Dokumen**

Menyisipkan beberapa dokumen ke dalam koleksi di MongoDB, kita menggunakan insert_many() metode.

Parameter pertama dari insert_many() metode ini adalah daftar yang berisi kamus dengan data yang ingin kita masukkan.

Contohnya

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]

daftarbuku = [
  { "namabuku": "React"},
  { "namabuku": "Node.js"},
  { "namabuku": "Django"},
  { "namabuku": "Git"},
  { "namabuku": "MongoDB"},
]

notes = mongocol.insert_many(daftarbuku)
print(notes.inserted_ids)
```

Metode insert_many() mengembalikan objek InsertManyResult, yang memiliki properti, inserted_ids, yang menyimpan id dari dokumen yang disisipkan.

- **Memasukkan Beberapa Dokumen dengan Membuat _id Tertentu**

Jika kita tidak ingin MongoDB menetapkan _id unik untuk dokumen kita, kita dapat menentukan _id nya saat kita memasukkan dokumen, dokumen tidak boleh memiliki _id yang sama.

Contohnya

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]

daftarbuku = [
  { "_id": 10, "namabuku": "React"},
  { "_id": 33, "namabuku": "Node.js"},
  { "_id": 11, "namabuku": "Django"},
  { "_id": 30, "namabuku": "Git"},
  { "_id": 37, "namabuku": "MongoDB"},
]

notes = mongocol.insert_many(daftarbuku)
print(notes.inserted_ids)
```

## **Memahami Cara Melakukan Find pada MongoDB**

Di MongoDB kita menggunakan metode find dan findOne untuk menemukan data dalam koleksi. Sama seperti pernyataan SELECT yang digunakan untuk mencari data dalam tabel di database MySQL.

- **Menemukan Satu Data pada MongoDB**

Memilih data dari koleksi di MongoDB, kita dapat menggunakan find_one() metode. Metode find_one() mengembalikan kemunculan pertama dalam seleksi.

Memukan dokumen pertama dalam koleksi `buku`.

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]

notes = mongocol.find_one()

print(notes)
```

- **Menemukan Semua Data pada MongoDB**

Memilih data dari tabel di MongoDB, kita juga bisa menggunakan find() metode. Metode find() mengembalikan semua kejadian dalam seleksi.

Parameter pertama dari find() metode ini adalah objek kueri. Dalam contoh ini kita menggunakan objek kueri kosong, yang memilih semua dokumen dalam koleksi.

Tidak ada parameter dalam metode find() yang memberi kita hasil yang sama seperti SELECT * di MySQL.

Mengembalikan semua dokumen dalam koleksi `buku`, dan cetak setiap dokumen.

Contohnya

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]

for notes in mongocol.find():
  print(notes)
```

- **Mengembalikan Hanya Beberapa Field**

Parameter kedua dari find() metode ini adalah objek yang menjelaskan field mana yang akan disertakan dalam hasil. Parameter ini opsional, dan jika dihilangkan, semua field akan disertakan dalam hasil.

Mengembalikan hanya nama dan bukan _id.

Contohnya

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]

for notes in mongocol.find({},{ "_id": 0, "namabuku": 1 }):
  print(notes)
```

Kita tidak diperbolehkan untuk menentukan nilai 0 dan 1 dalam objek yang sama (kecuali jika salah satu field adalah field _id). Jika kita menentukan field dengan nilai 0, semua bidang lainnya mendapatkan nilai 1, dan sebaliknya. Nilai 0 mengecualikan `_id` yang akan dikembalikan sedangkan nilai 1 akan mengembalikan `namabuku`.

## **Memahami Cara Melakukan Query pada MongoDB**

- **Memfilter Hasil**

Saat menemukan dokumen dalam koleksi, kita bisa memfilter hasilnya dengan menggunakan objek kueri. Argumen pertama dari find() metode ini adalah objek kueri, dan digunakan untuk membatasi pencarian.

Contoh menemukan dokumen dengan `namabuku` MongoDB.

Contohnya

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]

mongoquery = { "namabuku": "MongoDB" }

mongodoc = mongocol.find(mongoquery)

for notes in mongodoc:
  print(notes)
```

- **Kueri Lanjutan**

Membuat kueri tingkat lanjut, kita dapat menggunakan pengubah sebagai nilai dalam objek kueri. Misalnya untuk menemukan dokumen di mana field `namabuku` dimulai dengan huruf `N` atau lebih tinggi (berdasarkan abjad), gunakan pengubah lebih besar dari: {"$gt": "N"}.

Contoh menemukan dokumen `namabuku` dimulai dengan huruf `N` atau lebih tinggi.

Contohnya

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]

mongoquery = { "namabuku": { "$gt": "N" } }

mongodoc = mongocol.find(mongoquery)

for notes in mongodoc:
  print(notes)
```

- **Memfilter dengan Ekspresi Reguler**

Kita juga dapat menggunakan ekspresi reguler sebagai pengubah. Ekspresi reguler hanya dapat digunakan untuk query string.

Menemukan hanya dokumen di mana field `namabuku` dimulai dengan huruf `N`, gunakan ekspresi reguler {"$regex": "^N"}.

Contoh menemukan dokumen `namabuku` dimulai dengan huruf "N".

Contohnya

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]

mongoquery = { "namabuku": { "$regex": "^N" } }

mongodoc = mongocol.find(mongoquery)

for notes in mongodoc:
  print(notes)
```

## **Memahami Cara Melakukan Sort pada MongoDB**

- **Mengurutkan Hasil dari MongoDB**

Mengunakan sort() metode untuk mengurutkan hasil dalam urutan menaik atau menurun. Metode sort() ini mengambil satu parameter untuk nama field dan satu parameter untuk arah (naik adalah arah default).

Contoh mengurutkan hasil dari MongoDB menurut abjad berdasarkan `namabuku`.

Contohnya

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]

mongodoc = mongocol.find().sort("namabuku")

for notes in mongodoc:
  print(notes)
```

- **Mengurutkan Turunan dari MongoDB**

Mengunakan nilai -1 sebagai parameter kedua untuk mengurutkan secara descending.

- ascending

sort("namabuku", 1) 

- descending

sort("namabuku", -1)

Contoh mengurutkan hasil terbalik menurut abjad dengan `namabuku`.

Contohnya

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]

mongodoc = mongocol.find().sort("namabuku", -1)

for notes in mongodoc:
  print(notes)
```

## **Memahamai Cara Melakukan Delete pada MongoDB**

- **Menghapus Dokumen**

Menghapus satu dokumen, kita menggunakan delete_one() metode. Parameter pertama dari delete_one() metode ini adalah objek kueri yang menentukan dokumen mana yang akan dihapus. Jika kueri menemukan lebih dari satu dokumen, hanya kemunculan pertama yang dihapus.

Contoh menghapus dokumen dari `namabuku` "MongoDB".

Contohnya

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]

mongoquery = { "namabuku": "MongoDB" }

mongocol.delete_one(mongoquery)

for notes in mongocol.find():
  print(notes)
```

- **Menghapus Banyak Dokumen**

Menghapus lebih dari satu dokumen, gunakan delete_many() metode. Parameter pertama dari delete_many() metode ini adalah objek kueri yang menentukan dokumen mana yang akan dihapus.

Contoh Menghapus semua dokumen dari `namabuku` dimulai dengan huruf N.

Contohnya

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]

mongoquery = { "namabuku": {"$regex": "^N"} }

notes = mongocol.delete_many(mongoquery)

print(notes.deleted_count, "dokumen telah dihapus.")
```

- **Menghapus Semua Dokumen dalam Koleksi**

Menghapus semua dokumen dalam koleksi, teruskan objek kueri kosong ke delete_many() metode.

Contoh menghapus semua dokumen dalam koleksi `buku`.

Contohnya

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]

notes = mongocol.delete_many({})

print(notes.deleted_count, "dokumen telah dihapus.")
```

## **Memahamai Cara Melakukan Drop Collection pada MongoDB**

- **Menghapus Koleksi**

Kita dapat menghapus tabel, atau koleksi seperti yang disebut di MongoDB, dengan menggunakan drop() metode.

Contoh menghapus koleksi `buku`.

Contohnya

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]

mongocol.drop()
```

Metode drop() mengembalikan nilai true jika koleksi berhasil dijatuhkan, dan false jika koleksi tidak ada.

## **Memahamai Cara Melakukan Update pada MongoDB**

- **Memperbarui Koleksi**

Kita dapat memperbarui catatan, atau dokumen seperti yang disebut di MongoDB, dengan menggunakan update_one() metode. Parameter pertama dari update_one() metode ini adalah objek kueri yang menentukan dokumen mana yang akan diperbarui. Jika kueri menemukan lebih dari satu rekaman, hanya kemunculan pertama yang diperbarui. Parameter kedua adalah objek yang mendefinisikan nilai baru dari dokumen.

Contoh merubah `namabuku` dari `Django` menjadi `Node.js`.

Contohnya

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]

mongoquery = { "namabuku": "Django" }
mongonewvalues = { "$set": { "namabuku": "Node.js" } }

mongocol.update_one(mongoquery, mongonewvalues)

for notes in mongocol.find():
  print(notes)
```

- **Memperbarui Banyak Dokumen dari MongoDB**

Memperbarui semua dokumen yang memenuhi kriteria kueri, gunakan update_many() metode.

Contoh memperbarui semua dokumen dari `namabuku` dimulai dengan huruf `N`.

Contohnya

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]

mongoquery = { "namabuku": { "$regex": "^N" } }
mongonewvalues = { "$set": { "namabuku": "Laravel" } }

notes = mongocol.update_many(mongoquery, mongonewvalues)

print(notes.modified_count, "dokumen telah diperbarui.")
```

## **Memahami Cara Melakukan Limit pada MongoDB**

- **Membatasi Hasil dari MongoDB**

Membatasi hasil dari MongoDB, kita menggunakan limit() metode. Metode limit() ini mengambil satu parameter, angka yang menentukan berapa banyak dokumen yang akan dikembalikan.

Contoh membatasi hasil untuk hanya mengembalikan 3 dokumen dari MongoDB.

Contohnya

```
import pymongo

mongoclient = pymongo.MongoClient("mongodb://localhost:27017/")

db = mongoclient["perpustakaan"]

mongocol = db["buku"]

mongoresult = mongocol.find().limit(3)

for notes in mongoresult:
  print(notes)
```

# **Penutup**

Hai, aku Alpha Prima Galatheo Qallbu

Aku seorang Information Security Analyst, juga Pendiri di komunitas 1337Syndicate, aku suka budaya Jepang.
