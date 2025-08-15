# Git Multi-Commit Demo Steps

## 1. Lihat History Semua Branch

```bash
git log --graph --all --oneline
```

* Tunjukkan commit tree yang penuh cabang.
* Jelaskan tiap branch punya banyak commit bertahap.

---

## 2. Checkout & Bandingkan Isi Branch

```bash
git checkout feature-header-color
```

* Buka `style.css` di editor/browser → warna header berubah.

Lalu:

```bash
git checkout feature-footer
```

* Lihat ada footer tambahan dan styling berbeda.

---

## 3. Merge Branch Tanpa Konflik

Dari `main`:

```bash
git merge feature-header-color
```

* Harusnya langsung merge.
* Commit history tetap terlihat.

---

## 4. Merge Branch Dengan Konflik

Dari `main`:

```bash
git merge merge-conflict-demo
```

* Akan muncul merge conflict di `index.html`.
* Tunjukkan cara resolve:

  1. Buka file.
  2. Hapus tanda `<<<<<<<` dan `>>>>>>>`.
  3. Pilih versi yang mau dipakai.

```bash
git add index.html
git commit
```

---

## 5. Rebase Branch

```bash
git checkout rebase-demo
git rebase main
```

* Jelaskan commit di `rebase-demo` pindah ke ujung `main` → history jadi linear.

---

## 6. Cherry-pick Commit Spesial

Cari ID commit:

```bash
git log --oneline cherry-pick-demo
```

Ambil commit dengan pesan `SPECIAL cherry-pick commit`:

```bash
git checkout main
git cherry-pick <commit_id>
```

* Jelaskan hanya commit itu yang diambil tanpa merge semua perubahan branch.
