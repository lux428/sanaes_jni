# sanaes
Ini adalah library C/C++ AES256 yang telah dimodifikasi untuk digunakan pada Android NDK.
Tujuan dilakukan modifikasi library AES256 adalah untuk menghasilkan cypher text yang berbeda setiap kali proses enkripsi dilakukan.
Hal ini berhasil dilakukan dengan menambahkan variable random pada string/file yang akan di-enkripsi.

### Prerequisities
Android Studio with NDK SDK Tool ( CMake, LLDB, NDK)

### Installing
1. Taruh file berikut ke dalam folder "app\src\main\cpp"
```
aes.c
aes.h
base64.c
base64.h
CMakeList.txt
sanaes.c
sanaes.h
sanaes_jni.c
```
2. Pada file sanaes.h rubah definisi berikut sesuai dengan path yang akan digunakan
```
#define KEY_FILE_PATH   "sdcard/key.key"
```
3. Buat file key sesuai dengan KEY_FILE_PATH yang berisi 64byte key yang akan digunakan untuk encrypt dan decrypt. Setiap karakter dalam file ini haruslah representasi dari huruf-huruf hexadecimal (0-9 atau A-F). Kapital atau tidak adalah tidak masalah. Jumlahnya harus genap 64 karakter, tidak boleh kurang dan kelebihannya tidak akan dianggap.
```
0123456789abcdef0123456789ABCDEF0123456789abcdef0123456789ABCDEF
```
4. Pada file sanaes_jni.c rubah semua definisi fungsi agar sesuai dengan proyek android
```
Misal:
Java_com_example_hellojni_HelloJni_encryptString
menjadi:
Java_com_contoh_pabrik_Aplikasi_encryptString
```
5. Pada file .java yang akan memanggil library sanaes tambahkan kode berikut:
```
static {
    System.loadLibrary("sanaes");
}
public native String  encryptString(String plain);
public native String  decryptString(String cypher);
public native String  encryptFile(String plainDir, String cypherDir);
public native String  decryptFile(String cypherDir, String plainDir);
```
6. Pada build.gradle tambahkan kode berikut:
```
externalNativeBuild {
  cmake {
    path "src/main/cpp/CMakeLists.txt"
  }
}
```

### Usage
1. Encrypt String
```
String plain = "hello123";
String cypher = "";
cypher = encryptString(plain);
// bila hasil cypher = NULL berarti proses enkripsi gagal, penyebabnya adalah karena tidak adanya file .key
```
2. Decrypt String
```
String plain = "";
String cypher = "qlzRWXpPCHJpG304U5TyXg==";
plain = decryptString(cypher);
// bila hasil cypher = NULL berarti proses enkripsi gagal, penyebabnya adalah karena tidak adanya file .key
```
3. Encrypt File
```
String result = "";
result = encryptFile("sdcard/plain.txt", "sdcard/cypher.dat");
// hasil result adalah path yang file cypher yang dihasilkan. Dalam contoh ini, "sdcard/cypher.dat"
// bila hasil result = NULL berarti proses enkripsi gagal, penyebabnya adalah karena tidak adanya file .key
```
4. Decrypt File
```
String result = "";
result = decryptFile("sdcard/cypher.dat", "sdcard/decrypted.txt");
// hasil result adalah path yang file cypher yang dihasilkan. Dalam contoh ini, "sdcard/decrypted.txt"
// bila hasil result = NULL berarti proses enkripsi gagal, penyebabnya adalah karena tidak adanya file .key
```

### Author
* **Lukman Sahid** - *Solusi Antar Network*
