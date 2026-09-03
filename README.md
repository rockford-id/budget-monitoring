# Mockup Budget Monitoring — SMRA

Mockup interaktif modul Budget Monitoring, memakai design system **Portal v3** (shared-ui v2.0.0).
Semua data di dalamnya **data demo**, bukan data produksi.

- 7 grup menu / 31 layar
- Acuan skema: 21 tabel (`bm_*`), revisi 2026-08-06
- File utama: `index.html` (satu file + folder `assets/`)

## Membuka secara lokal

Harus lewat server lokal — dibuka langsung dengan `file://` sebagian aset bisa diblokir browser.

```bash
# dari folder ini
python -m http.server 8799        # atau
npx --yes http-server -p 8799
```

Lalu buka <http://localhost:8799/>

## Menerbitkan ke GitLab Pages

Sudah diatur di `.gitlab-ci.yml`. Setiap push ke branch utama akan menerbitkan ulang.

1. Buat project baru di GitLab, lalu:

   ```bash
   git init
   git add .
   git commit -m "Mockup Budget Monitoring v2 — skema 21 tabel"
   git branch -M main
   git remote add origin <URL-REPO-GITLAB>
   git push -u origin main
   ```

2. Tunggu pipeline selesai (**Build → Pipelines**).
3. URL-nya ada di **Deploy → Pages**.

### Membatasi siapa yang boleh membuka

Default GitLab Pages bisa diakses siapa saja yang punya URL-nya. Untuk membatasi ke
anggota project saja:

**Settings → General → Visibility, project features, permissions → Pages** → pilih
*"Only project members"* (butuh Pages access control aktif di instance GitLab-nya).

### Kalau pipeline gagal

- **`cp: not found` / error shell** — runner-nya Windows/shell, bukan Docker. Ganti
  `script:` dengan perintah PowerShell (`New-Item`, `Copy-Item`), atau pakai runner Docker.
- **Job `pages` tidak jalan** — cek branch utamanya memang bernama sesuai
  `CI_DEFAULT_BRANCH` (`main`/`master`), dan GitLab Pages aktif di instance tersebut.
- **Halaman terbit tapi tampilan rusak** — folder `assets/` tidak ikut ter-commit.
  Pastikan `git status` tidak mengabaikannya.

## Catatan

- Aset design system di `assets/portal-ui/` disalin verbatim dari
  `portal_v3_app/shared-ui-package/resources/dist/`. Kalau design system di-update,
  salin ulang folder itu.
- Data seluruhnya in-memory: refresh halaman = kembali ke kondisi awal.
