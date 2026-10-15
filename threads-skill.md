---
name: threads-content-from-link
description: "Create engaging Threads posts from article/news URLs, or rewrite X/Twitter posts into Indonesian Threads. Extracts content, generates concise and viral-style Threads posts."
version: 1.2.0
author: Arsy Opraza + Hermes
tags: [threads, content-creation, social-media, article-summary]
aliases: [threads-content]
---

# Threads Content from Link

Membuat konten Threads dari referensi artikel, berita, atau halaman web. Tinggal kasih URL, langsung jadi post Threads yang engaging.

## Trigger

## Shortcut: /threads-content

Skill ini bisa dipanggil lewat command shortcut `/threads-content` atau `threads-content` di awal pesan. Ketika user mengirim command tersebut dengan URL, langsung proses tanpa tanya-tanya lagi: ambil URL → load skill ini → generate konten → kasih output.

**Alias:** `threads-content` adalah alias yang me-load skill ini dan langsung memproses URL tanpa konfirmasi. Tidak perlu prompt terpisah.

## Trigger
- Mengirim URL artikel/berita dan minta dibuatkan konten Threads
- Mengirim URL tweet/X dan minta direwrite jadi konten Threads Bahasa Indonesia
- Menyebut "threads", "konten threads", "bikinin post threads dari link", "rewrite ke indo"
- Ada di topic "content creation" dan kirim link
- Minta konten tentang topik tertentu (tanpa URL, tanpa draft) → pakai ## Alternative B: Topic-Based Content

PENTING: Bedakan trigger artikel vs Twitter. Twitter = langsung ke ## Twitter/X Post Rewrite workflow.

## Workflow

### Step 1: Ekstrak konten dari URL

Gunakan web search / content extraction untuk mengambil isi artikel:

```
web_extract url: <URL yang diberikan user>
```

Kalau link banyak, ekstrak satu per satu.

### Step 2: Analisis & identifikasi angle

Dari konten yang diekstrak, identifikasi:
- **Key takeaway** (poin paling penting)
- **Fakta mengejutkan** atau data menarik
- **Sudut pandang unik** yang bisa jadi hook
- **Target audience** — siapa yang peduli sama info ini?

### Step 3: Generate Threads posts

Threads punya karakteristik khusus:
- **Maksimal 500 karakter per post** (aman di 400-480)
- **Hook kuat di awal** — 1-2 kalimat pertama harus bikin orang berhenti scroll
- **Gaya santai & conversational** — bukan formal kayak LinkedIn
- **Emoji dipakai natural** — 1-3 per post, jangan lebay
- **1-2 hashtag relevan** di akhir (contoh: #Tech #AI)
- **Line breaks** buat readable — jangan blok teks padat

Format output:

```
🧵 [Hook menarik — bikin penasaran]

[Isi — 2-4 kalimat yang ngejelasin poin utama dengan gaya santai]

[Call-to-action atau insight penutup]

#️⃣ #Hashtag1 #Hashtag2
```

Kalau kontennya panjang dan perlu dipecah jadi thread bersambung (TANPA numbering — pisahin post pake double line break aja):

```
🧵 [Hook]

[Isi part 1]


[Isi part 2 — gak usah pake "🧵 2/3"]


[Penutup + CTA, gak usah numbering]

#️⃣ #Hashtag
```

User prefer output clean tanpa "🧵 1/3, 2/3" — langsung bisa copy-paste ke Threads. Double line break antar post udah cukup jadi pemisah.

### Step 3.5: Auto LLM Judge (quality gate)

Setelah generate konten, **otomatis** evaluasi pake skill `llm-judge` TANPA diminta user:

1. Load skill `llm-judge` dengan `skill_view(name='llm-judge')`
2. Evaluasi konten yang baru digenerate pake rubric 6 dimensi
3. Output berdasarkan verdict:

| Verdict | Action |
|---------|--------|
| ✅ APPROVED (≥7.5) | Lanjut ke Step 4, kasih badge ✅ di output |
| 🔄 NEEDS_REVISION (6.0-7.4) | Kasih konten + masalah + saran revisi ke user |
| ❌ REJECTED (<6.0) | Auto rewrite dengan angle berbeda → judge ulang (max 2x) |

Kalau setelah 2x rewrite masih REJECTED, stop dan tanya user: "2x rewrite masih belum tembus. Lanjut atau skip?"

### Step 4: Review & kasih ke user

Tampilkan hasil akhir dengan opsi:
- Copy-paste langsung (format plain text)
- Minta revisi (user kasih feedback)
- Generate versi alternatif (angle berbeda)

## Twitter/X Post Rewrite

Ketika user kirim URL tweet/X dan minta dibuatkan konten Threads (bukan dari artikel). Ini workflow terpisah dari artikel.

### Perbedaan utama dengan artikel

| Aspek | Artikel | Twitter Rewrite |
|-------|---------|-----------------|
| Jumlah post | 3-4 post thread | **1 post** (kecuali user minta lebih) |
| Gaya | Bebas eksplorasi, kasih opini | **Ikutin struktur original, beda kata** |
| CTA/penutup | Tambahin insight/ajakan | Jangan ditambahin kecuali user minta |
| Emoji | Natural 1-3 per post | Hanya kalau emang perlu |
| Output | Satu versi final | **Langsung kasih 3 opsi sekaligus** |

### Workflow Twitter Rewrite

**Step 1: Ambil konten tweet**
Gunakan `xurl read <TWEET_ID>` kalau xurl tersedia. Kalau gak ada xurl, load skill `x-tweet-extract` dan ikutin workflow-nya — syndication endpoint (`cdn.syndication.twimg.com/tweet-result?id=...`) selalu lebih reliable daripada oEmbed. Kalau tweet-nya thread atau nyantolin link artikel, ambil semua konteksnya.

**Step 2: Pahami struktur tweet original**
Identifikasi pola tweet-nya:
- Hook pembuka apa?
- Kontras/perbandingan apa?
- Detail teknis/fakta apa?
- Penutup apa?

Struktur inilah yang HARUS dipertahankan di rewrite.

**Step 3: Generate 3 opsi langsung**
Kasih 3 opsi dengan angle berbeda, semua **1 post**. Tiap opsi:
- Struktur mengikuti tweet original
- Kata-kata fresh, bukan sinonim doang
- Gak nambahin opini, komentar pribadi, atau CTA
- Bahasa Indonesia natural
- 1-2 hashtag di akhir

**Step 4: User pilih atau revisit**
Setelah user pilih -> "Aman?" untuk final approval.
Kalau user minta "Opsi lain" -> generate 3 opsi baru dengan angle beda.
Kalau user minta revisi spesifik -> revisi opsi yang dipilih.

### Contoh Twitter Rewrite

Tweet original (EN):
> "OpenAI just dropped o3. It reasons for HOURS before answering. Not a chatbot — a research partner."

3 opsi:

```
🧵 OpenAI baru aja nge-drop o3. Model ini mikir BERJAM-JAM sebelum ngejawab.

Bukan chatbot biasa — ini partner riset.

#AI #OpenAI
```

```
🧵 o3 dari OpenAI bukan sekadar chatbot. Dia reasoning dulu berjam-jam sebelum kasih jawaban.

Lebih mirip rekan riset ketimbang asisten.

#AI #OpenAI
```

```
🧵 Berjam-jam mikir sebelum jawab. Itu yang bikin o3 beda dari chatbot manapun.

OpenAI gak lagi bikin asisten — mereka bikin partner riset.

#AI #OpenAI
```

## Alternative A: Draft Rewrite (no URL)

Ketika user kirim teks mentahan (bukan URL) dengan format "ubah draft ini: [teks]" — ini bukan artikel dan bukan Twitter. User udah punya ide sendiri, cuma minta dipoles jadi konten Threads.

Workflow:
1. Ambil teks mentahan user sebagai sumber
2. Identifikasi angle/tema (ini penting karena user udah pilih angle sendiri)
3. Poles jadi konten Threads — tambahin hook, flow, emoji, hashtag
4. Tetap kasih opini/CTA karena ini konten orisinal user
5. Judge + final seperti biasa

Jangan ganti angle user. Kalau dia nulis tentang "AI unlimited", jangan diubah jadi "AI pricing model" — pertahanin topik yang dia pilih.

## Alternative B: Topic-Based Content (no URL, no draft)

Ketika user minta konten tentang topik tertentu tanpa ngasih URL atau draft — misal "buat konten tentang monetisasi FB buat pemula". Ini bukan artikel, bukan tweet, bukan draft — user cuma ngasih tema.

### Workflow

**Step 1: Verifikasi knowledge**
Topik user bisa jadi hal yang knowledge-nya udah basi atau kurang akurat. Sebelum nulis:
- Kalau topiknya menyangkut **kebijakan/platform yang sering berubah** (aturan monetisasi, update fitur, harga layanan) — coba cek dulu lewat web/browser.
- Kalau sumber terblokir (bot detection, login wall) dan gak bisa diverifikasi — **jangan asumsi**. Langsung bilang ke user jujur.

**Step 2: Tentukan angle**
Dari tema yang dikasih user, pilih 2-3 angle berbeda yang engaging:
- **Myth debunking** — bongkar miskonsepsi umum
- **Step-by-step praktis** — panduan langsung bisa dipraktekin
- **Realis + honesty** — kasih ekspektasi realistis, include jebakan umum

**Step 3: Generate 3 opsi**
Sama kayak Twitter rewrite: langsung kasih 3 opsi sekaligus. Tiap opsi pake angle berbeda. Format: satu post (bisa jadi thread 2-3 post panjang) gaya Threads Indonesia.

**Step 4: Factual disclaimer**
Kalau ada klaim spesifik yang gak bisa diverifikasi karena keterbatasan akses, tambahin catatan ke user: "ini based on info yang gue punya, [poin X] perlu dicek lagi ya."

**Step 5: Judge + final**
LLM Judge seperti biasa. Tapi untuk konten tipe edukasi/informasional, accuracy score dikasih bobot lebih tinggi — klaim yang gak bisa diverifikasi otomatis nurunin skor accuracy.

## Pitfalls

- **Jangan copy-paste dari artikel** — paraphrase dan kasih perspektif baru
- **X Articles (Twitter Notes) gak bisa discrap** — syndication endpoint cuma return metadata (title, preview, author), bukan full body. Redirect ke user: minta copy-paste full text, screenshot (OCR), atau skip. Jangan looping retry.
- **Cek fakta** — jangan klaim data yang gak ada di sumber
- **Jangan clickbait bohong** — hook harus akurat dengan isi artikel
- **Unverifiable claims** — kalau bot detection / login wall / paywall ngeblok verifikasi, JANGAN asumsikan fakta masih valid. Bilang jujur ke user. Jangan publish klaim yang gak bisa dibuktikan, apalagi untuk konten edukasi/informasional.
- **Topic-based content disclaimer** — untuk konten yang menyangkut kebijakan platform yg sering berubah (aturan monetisasi FB/TikTok, harga layanan, update fitur), selalu tambahin catatan kalau ada poin yang gak bisa diverifikasi. Lebih baik transparent daripada nyebarin info basi.
- Link Threads gak support formatting (bold/italic), jadi plain text aja
- Kalau artikel berbahasa Inggris, hasilkan konten Threads dalam **Bahasa Indonesia** (kecuali user minta English)
- **Untuk Twitter rewrite: JANGAN tambahin opini, komentar pribadi, atau CTA** — murni rewrite struktur original. User akan minta kalau mau ditambahin.
- **Untuk Twitter rewrite: DEFAULT 1 POST** — jangan bikin thread kecuali user eksplisit minta. Tweet itu pendek, Threads post 500 karakter udah cukup.
- **Untuk Twitter rewrite: langsung kasih 3 opsi** — jangan satu-satu, biar user bisa langsung bandingin dan pilih.
- **Output akhir HARUS clean & siap copy** — gak pake "🧵 1/3, 2/3" numbering. Pisahin post cukup dengan double line break. User (Arsy) prefer langsung tap-copy-paste tanpa perlu ngedit lagi.

## Contoh

Input: link artikel tentang "Google merilis Gemini 2.5"

Output:
```
🧵 Google baru aja drop Gemini 2.5 dan ini BUKAN update kecil-kecilan.

Yang bikin gila: reasoning ability-nya naik 40% dibanding versi sebelumnya. Mereka klaim ini model paling pintar yang pernah mereka buat — bahkan ngalahin GPT-4 di benchmark coding.

Yang paling menarik buat gue: Gemini 2.5 bisa reasoning sampe 1 juta token context window. Bayangin baca 10 novel terus ngejawab pertanyaan detail tentang semuanya. 🤯


Ini bukan sekadar "update model" — ini sinyal kalau Google serius banget di AI race. Dan timing-nya pas banget seminggu sebelum OpenAI rilis sesuatu.

Kira-kira OpenAI bakal counter pake apa ya? 👀

#AI #GoogleGemini
```
