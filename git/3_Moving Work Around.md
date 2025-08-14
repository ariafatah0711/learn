# Moving Work Around
## 1. Git Cherry-pick
Sampai sejauh ini, kita sudah membahas dasar-dasar Git — mulai dari membuat commit, membuat branch, hingga berpindah-pindah di dalam source tree. Hanya dengan konsep-konsep ini saja, sebenarnya kita sudah bisa memanfaatkan sekitar 90% kekuatan Git untuk kebutuhan sehari-hari developer.

Namun, ada 10% sisanya yang sering kali berguna untuk workflow yang lebih kompleks — atau ketika kita berada dalam situasi "terjebak" dan perlu solusi cepat. Bagian ini membahas konsep “moving work around” — cara memindahkan pekerjaan (commit) ke tempat yang kita inginkan, dengan fleksibel dan tepat sasaran.

#### Git Cherry-pick
Perintah pertama yang akan kita bahas adalah git cherry-pick. Bentuk dasarnya seperti ini:
```bash
git cherry-pick <Commit1> <Commit2> <...>
```

Sederhananya, cherry-pick digunakan untuk menyalin satu atau beberapa commit tertentu dari tempat lain, lalu menempelkannya tepat di bawah posisi HEAD saat ini.

Kelebihannya, cherry-pick sangat mudah dipahami — tidak ada "sulap-sulap" seperti yang mungkin terasa pada rebase. Kita hanya memilih commit yang diinginkan, dan Git akan menambahkannya di branch kita saat ini.

### visual
Misalnya, kita punya repository dengan 3 branch. Di branch side, ada beberapa commit yang ingin kita bawa ke branch main.
Hal ini memang bisa dilakukan dengan rebase (yang sudah kita pelajari sebelumnya), tapi kita akan coba melakukannya dengan cherry-pick.

![alt text](<images/3_Moving Work Around/image.png>)

Di sini, kita ingin mengambil commit C3, C4, dan C7 dari branch lain, lalu menambahkannya ke branch main. Caranya:
```bash
git cherry-pick C3 C4 C7
```

Hasilnya? Git langsung menambahkan commit-commit yang kita pilih ke branch main tanpa memindahkan commit lain.
Praktis, cepat, dan aman.

![alt text](<images/3_Moving Work Around/image-1.png>)

---

## 2. Git Interactive Rebase
git cherry-pick memang luar biasa kalau kita sudah tahu persis commit mana yang ingin diambil (dan tahu hash-nya).
Tapi bagaimana kalau kita belum tahu commit mana yang dibutuhkan?

Tenang, Git punya fitur yang pas untuk itu — interactive rebase.
Dengan fitur ini, kita bisa meninjau commit-commit yang akan dipindahkan, mengatur urutannya, atau bahkan menghapus commit yang tidak diperlukan sebelum proses rebase berlangsung.

#### Dasar Interactive Rebase
Interactive rebase dijalankan dengan menambahkan opsi -i pada perintah git rebase.

```bash
git rebase -i <target>
```

Jika kita menambahkan opsi ini, Git akan membuka sebuah “UI” (biasanya berupa file di text editor seperti vim, nano, atau editor default Git) yang berisi daftar commit yang akan dipindahkan.
Di dalam daftar tersebut, kita bisa melihat hash commit dan pesan commit-nya — sangat membantu untuk memastikan commit yang kita pilih benar.

#### Apa yang Bisa Dilakukan?
Dalam mode interactive rebase, kita bisa:
- Mengubah urutan commit — cukup ubah susunan barisnya sesuai urutan yang diinginkan.
- Menghapus commit tertentu — dengan mengganti kata pick menjadi drop (atau menghapus baris commit tersebut).
> Catatan: Di dunia nyata, interactive rebase juga memungkinkan hal-hal seperti squash (menggabungkan commit), amend pesan commit, bahkan edit isi commit itu sendiri. Tapi di sini kita fokus pada dua operasi utama: mengubah urutan dan menghapus commit.

### visual
![alt text](<images/3_Moving Work Around/image-2.png>)

Misalnya, kita ingin mengatur ulang urutan 4 commit terakhir. Kita jalankan:
```bash
git rebase -i HEAD~4
```

![alt text](<images/3_Moving Work Around/image-3.png>)

Git akan membuka daftar commit. Kita ubah urutannya menjadi:
```bash
pick C3
pick C5
pick C4
drop C2
```

![alt text](<images/3_Moving Work Around/image-4.png>)

Hasilnya: commit C3, C5, dan C4 akan tetap ada di urutan yang kita tentukan, sedangkan C2 akan dihapus dari sejarah branch.

Dengan interactive rebase, kita punya kendali penuh atas “alur cerita” commit di branch kita.
Jadi kalau urutan commit di repo terasa berantakan, fitur ini adalah “editor naskah” terbaik yang bisa kita gunakan.
