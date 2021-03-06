# Membangun petunjuk (Linux)

Ikuti panduan di bawah ini untuk membangun Elektron di Linux.

## Prasyarat

- macOS > = 10.11.6
- [@googlechrome](https://developer.apple.com/technologies/tools/) pada Twitter
- [node.js](http://nodejs.org) untuk Linux)

Jika Anda menggunakan Python yang didownload oleh Homebrew, Anda juga perlu menginstal modul Python berikut ini:

- [pyobjc](https://pythonhosted.org/pyobjc/install.html)

## macos SDK

Jika Anda hanya mengembangkan Elektron dan tidak berencana untuk mendistribusikan ulang custom Electron build, Anda bisa melewati bagian ini.

Untuk fitur tertentu (misalnya pinch-zoom) agar berfungsi dengan benar, Anda harus menargetkan SDK macos 10.10.

Resmi membangun elektron yang dibangun dengan [Xcode 8.2.1](http://adcdownload.apple.com/Developer_Tools/Xcode_8.2.1/Xcode_8.2.1.xip), yang tidak mengandung 10.10 SDK secara default. Untuk mendapatkannya, pertama download dan mount [Xcode 6.4](http://developer.apple.com/devcenter/download.action?path=/Developer_Tools/Xcode_6.4/Xcode_6.4.dmg) DMG.

Kemudian, dengan asumsi bahwa Xcode 6.4 DMG telah dipasang `pada/volume/Xcode` dan bahwa Anda Xcode 8.2.1 menginstal adalah di `/Applications/Xcode.app`, jalankan:

```sh
cP - r /Volumes/Xcode/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.10.sdk /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/
```

Anda juga perlu mengaktifkan Xcode untuk membangun terhadap 10.10 SDK:

- Buka `/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Info.plist`
- Mengatur `MinimumSDKVersion` ke `10.10`
- Simpan file

## Dapatkan kode

```sh
$ git klon https://github.com/electron/electron
```

## Bootstrapping

Skrip bootstrap akan men-download semua dependensi diperlukan membangun dan menciptakan membangun project file. Perhatikan bahwa kita menggunakan  ninja </ 0> untuk membangun Elektron sehingga tidak ada proyek Xcode yang dihasilkan.</p> 

```sh
$ cd elektron
$ ./script/bootstrap.py -v
```

## Bangunan 

Jika Anda ingin membangun target ` Release ` dan ` Debug `:

```sh
$ ./script/build.py
```

Anda juga dapat membangun target ` Debug ` saja:

```sh
$ ./script/build.py -c R
```

Setelah selesai, Anda bisa menemukan biner debug ` elektron ` di bawah ` keluar / D `.

## Dukungan 32bit

Elektron hanya bisa dibangun untuk target 64bit pada macOS dan tidak ada rencana untuk melakukannya dukung macos 32bit di masa depan.

## Membersihkan

Untuk membersihkan bangunan file:

```sh
$ npm bersih
```

Untuk pembersihan hanya `keluar` dan `dist` direktori:

```sh
$ npm berjalan bersih-bangun
```

**Catatan:** Kedua perintah bersih perlu menjalankan `bootstrap` lagi sebelum membangun.

## Coba

Lihat [Bangun Gambaran Sistem: Pengujian](build-system-overview.md#tests)