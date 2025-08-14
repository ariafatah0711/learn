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
WOAHHHhhh Nelly! We have quite the goal to reach in this level.

Here we have main that is a few commits ahead of branches one two and three. For whatever reason, we need to update these three other branches with modified versions of the last few commits on main.

Branch one needs a re-ordering of those commits and an exclusion/drop of C5. Branch two just needs a pure reordering of the commits, and three only needs one commit transferred!

We will let you figure out how to solve this one -- make sure to check out our solution afterwards with show solution.

### solve
![alt text](<images/5_Advanced Topics/image-4.png>)

```bash
git checkout one
git cherry-pick C4 C3 C2
```
![alt text](<images/5_Advanced Topics/image-5.png>)

```bash
git checkout two
git cherry-pick C5 C4 C3 C2
```
![alt text](<images/5_Advanced Topics/image-6.png>)

```bash
git rebase C2 three
```
![alt text](<images/5_Advanced Topics/image-7.png>)
