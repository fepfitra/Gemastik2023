# Writeup

> TLDR; Terdapat null-byte injection pada ProcessBuilder karena implementasi javanya bergantung OS.

1. Buat akun dengan format `../../bin/sh\0<randomstring>`.
2. Kemudian login dengan dua sesi berbeda.
3. Pada sesi kedua, lakukan penghapusan akun.
4. Pada sesi pertama, lakukan eksekusi tools dengan mengupload sebuah script bash.
    Eksekusi bash script dapat dilakukan karena data user sudah tidak ada di database, maka folder akan mengikuti username.
    Lalu mengingat OS yang digunakan adalah debian, sedangkan debian menggunakan C, maka program akan membaca hingga null-byte, sehingga didapatkan program yang harus dieksekusi adalah `/opt/user_data/../../bin/sh` atau `/bin/sh`.

Perlu dicatat bahwa tidak ada network agent yang dapat digunakan, sehingga harus menginject network agent dari bash script yang dijalankan pula.

## How to use solver
> Solver memanfaatkan query parameter pada HTTP untuk eksfiltrasi data

1. Ubah link webhook dan command RCE yang diinginkan pada `script.sh`
2. Ubah link web target pada `solver.py`
3. Jalankan `solver.py`, kemudian nanti akan muncul sebuah request pada webhook dengan query berupa base64 hasil dari command RCE yang dieksekusi.