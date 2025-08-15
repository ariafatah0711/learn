# Advanced Topics
## 1. Rebasing Multiple Branches
Di skenario ini, kita punya banyak branch yang masing-masing punya commit berbeda. Tugas kita adalah merebase semua branch tersebut ke main dan mengurutkan commit secara sekuensial:

### solve
![alt text](<images/5_Advanced Topics/image.png>)

```bash
# Rebase branch bugFix ke main
git rebase main bugFix

# Rebase branch another ke branch side
git rebase side another

# Rebase bugFix ke posisi after another
git rebase bugFix

# Terakhir, rebase another ke main
git rebase another main
```

![alt text](<images/5_Advanced Topics/image-1.png>)

---

## 2. Specifying Parents
Di Git, tanda ^ dan ~ punya fungsi berbeda:
- ~<n> → mundur <n> generasi ke belakang dari commit sekarang (mengikuti satu jalur utama).
- ^<n> → memilih parent ke-n dari sebuah merge commit.
  - ^ tanpa angka = parent pertama (garis lurus ke atas pada visual)
  - ^2 = parent kedua (biasanya cabang lain yang di-merge)

Contoh:
- main^ → commit parent pertama dari main
- main^2 → commit parent kedua dari main
- Modifiers bisa dikombinasikan:
  - main~2^2~1 → melompat antar parent dan generasi dalam satu perintah

### solve
![alt text](<images/5_Advanced Topics/image-2.png>)

Tujuannya: buat branch baru (bugWork) di posisi tertentu menggunakan modifier ^ dan ~ tanpa langsung menyebut hash commit.

```bash
git checkout HEAD~^2~1
git branch bugWork
git checkout main
```
atau gunakan 1 inline
```bash
git branch bugWork main^^2^
```

![alt text](<images/5_Advanced Topics/image-3.png>)

---

## 3. Branch Spaghetti
Di sini kita punya situasi “spaghetti branch” — branch main sudah beberapa commit lebih maju dibandingkan branch one, two, dan three.
Tugas kita: memperbarui ketiga branch tersebut, tapi tiap branch punya kebutuhan yang berbeda:

- Branch one → butuh commit dari main tapi tanpa C5 dan urutannya diubah.
- Branch two → butuh semua commit dari main tapi urutannya diubah.
- Branch three → cuma butuh satu commit dari main.

### solve
![alt text](<images/5_Advanced Topics/image-4.png>)

Kita ambil commit dari main (C4, C3, C2) sesuai urutan yang diinginkan, skip C5:
```bash
git checkout one
git cherry-pick C4 C3 C2
```
![alt text](<images/5_Advanced Topics/image-5.png>)

Kita ambil semua commit dari main, tapi dengan urutan khusus:
```bash
git checkout two
git cherry-pick C5 C4 C3 C2
```
![alt text](<images/5_Advanced Topics/image-6.png>)

Karena three cuma butuh commit C2, kita bisa langsung rebase ke C2:
```bash
git rebase C2 three
```

![alt text](<images/5_Advanced Topics/image-7.png>)
