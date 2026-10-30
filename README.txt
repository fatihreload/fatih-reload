FATIH RELOAD - VERSI TERHUBUNG SUPABASE

1. Upload isi folder/ZIP ini ke GitHub repository yang digunakan Cloudflare Pages.
2. Supabase Project URL yang dipakai:
   https://rxobthqkwhyfmcduuzeu.supabase.co
   Publishable key ditanam di frontend (memang dirancang untuk penggunaan publik).
   JANGAN pernah menaruh service_role key di index.html atau GitHub.

3. Agar katalog paket bisa dibaca pengunjung, jalankan SQL berikut di Supabase SQL Editor:

   grant select on table public.packages to anon, authenticated;
   create policy "Public can read active packages"
   on public.packages
   for select
   to anon, authenticated
   using (is_active = true);

   Jika policy dengan nama sama sudah ada, jangan buat ulang; cukup pastikan grant dan policy aktif.

4. Login admin:
   Dashboard Admin -> masukkan EMAIL dan PASSWORD akun admin Supabase Auth.
   (Username tampilan "admin" belum dipetakan; Supabase Auth login memakai email/password.)

5. Impor voucher:
   Pilih paket, unggah CSV satu kode per baris, lalu tekan Impor.
   Fungsi Edge Function `import-vouchers` harus sudah di-deploy.
   Pastikan fungsi tersebut memiliki secret/env SUPABASE_URL, SUPABASE_ANON_KEY,
   dan SUPABASE_SERVICE_ROLE_KEY. Service role hanya boleh berada di sisi server.
   Pada fungsi ini, Verify JWT with legacy secret perlu dimatikan jika kode fungsi
   melakukan verifikasi token manual seperti yang sudah dibuat sebelumnya.

CATATAN PENTING
- Katalog paket dan login admin sudah terhubung ke Supabase.
- Halaman pelanggan belum membuat order ataupun menerima pembayaran.
- QRIS dinamis membutuhkan integrasi payment gateway dan webhook yang memverifikasi pembayaran.
- Pengiriman voucher otomatis membutuhkan alur alokasi voucher setelah webhook pembayaran sah.
- WhatsApp otomatis membutuhkan WhatsApp Business API/provider.
- Jangan menerima uang atau menjual voucher melalui tombol ini sebelum pembayaran dan webhook selesai diuji.
