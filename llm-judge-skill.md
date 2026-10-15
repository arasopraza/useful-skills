---
name: llm-judge
description: "LLM-as-Judge: evaluasi kualitas konten (Threads, ebook, dokumen) + filter otomatis. Score multi-dimensi dan beri verdict."
version: 1.1.0
author: Arsy Opraza + Hermes
tags: [evaluation, quality, judge, filtering, threads, documents, review]
---

# LLM Judge — Evaluasi & Filter Konten

Skill untuk mengevaluasi kualitas konten menggunakan LLM sebagai judge — mencakup konten Threads, ebook, dokumen, dan teks lainnya. Dipakai sebagai quality gate.

## Trigger

Gunakan skill ini ketika:
- User minta "review konten", "cekin kualitas", "judge this", "nilaiin"
- User minta "review ebook/dokumen/PDF ini" atau "Act as llm as judge"
- Setelah generate konten Threads — **auto-trigger** sebagai quality gate
- User kirim teks dan minta evaluasi publish/tidak
- User minta filter konten dari batch hasil generate

**PENTING:** Deteksi jenis konten dulu sebelum evaluasi. Threads vs dokumen/ebook punya rubric & threshold berbeda.

## Dimensi Penilaian — Threads (Skala 1-10)

| # | Dimensi | Pertanyaan Kunci |
|---|---------|-----------------|
| 1 | 🎣 **Hook Strength** | Kalimat pertama bikin penasaran & berhenti scroll? |
| 2 | 💡 **Clarity** | Pesan jelas? Gak ambigu? Gak butuh baca ulang? |
| 3 | 🔥 **Engagement** | Memancing interaksi (reply, repost, like)? Ada emotional pull? |
| 4 | ✅ **Accuracy** | Fakta akurat? Gak ada klaim palsu atau misleading? |
| 5 | ✍️ **Language** | Bahasa Indonesia natural? Flow enak? Gak kayak translate-an? |
| 6 | 📏 **Format** | ≤500 karakter per post? Line breaks proper? Hashtag relevan 1-2? |

## Dimensi Penilaian — Dokumen/Ebook (Skala 1-10)

| # | Dimensi | Pertanyaan Kunci | Bobot |
|---|---------|-----------------|-------|
| 1 | 📖 **Content Quality** | Substansi kuat? Kedalaman cukup? Bukan filler? | 25% |
| 2 | 🏗️ **Structure & Flow** | Alur logis? Urutan bab masuk akal? Gak loncat-loncat? | 20% |
| 3 | ✅ **Accuracy** | Fakta, command, harga, klaim — semuanya akurat? | 20% |
| 4 | ✍️ **Language & Tone** | Konsisten? Sesuai target audiens? Bebas AI-isms? | 15% |
| 5 | 📋 **Completeness** | Semua yang dijanjikan ada? Gak bolong di tengah? | 10% |
| 6 | 🎯 **Targeting** | Tepat sasaran buat audiens yang dituju? | 10% |

**Perbedaan threshold dokumen vs Threads:** Dokumen threshold completeness lebih tinggi (pembaca ekspektasi utuh), dan accuracy dicek lebih ketat (klaim harga, command, versi — harus akurat).

## Threshold & Verdict

| Rata-rata Score | Verdict | Artinya |
|-----------------|---------|---------|
| ≥ 7.5 | ✅ **APPROVED** | Siap publish, langsung kasih ke user |
| 6.0 – 7.4 | 🔄 **NEEDS_REVISION** | Kasih saran revisi, user putuskan |
| < 6.0 | ❌ **REJECTED** | Jangan publish. Auto rewrite dengan angle beda. |

**Safety override:** Meskipun score APPROVED, kalau ada **accuracy issue serius** (klaim palsu, fakta ngawur), auto-downgrade ke REJECTED.

## Workflow

### Mode 1: Judge Manual (user minta review)

1. User kasih konten → agent evaluasi pake rubric di atas
2. Agent bertindak sebagai judge LLM — baca konten, nilaiin tiap dimensi
3. Hitung rata-rata → tentukan verdict
4. Output dalam format standar (lihat ## Format Output)
5. Kalau bukan APPROVED, kasih saran revisi yang **spesifik & actionable**

### Mode 2: Auto-Filter Pipeline (dipanggil setelah generate konten)

1. Setelah `threads-content-from-link` generate konten
2. **Otomatis** jalankan judge tanpa diminta user
3. Output APPROVED → langsung kasih ke user dengan verdict badge
4. Output NEEDS_REVISION → kasih konten + breakdown masalah + saran
5. Output REJECTED → **auto rewrite** dengan angle berbeda, lalu judge ulang

### Mode 4: Document/Ebook Review (user minta review dokumen)

1. Ekstrak teks dari dokumen (pakai `ocr-and-documents` skill untuk PDF, atau `python-docx` untuk DOCX)
2. Tentukan jenis dokumen (ebook edukasi, laporan, artikel panjang, dll) — ini mempengaruhi bobot dimensi
3. Evaluasi pakai rubric dokumen (lihat ## Dimensi Dokumen)
4. Output dalam format standar dokumen
5. Breakdown WAJIB diberikan untuk semua verdict (beda dengan Threads yang APPROVED cukup ringkas)
6. Kalau ada mismatch struktural (misal Daftar Isi vs isi), **tanya user dulu sebelum judge** — bisa jadi artefak lama yang belum diupdate, bukan kesalahan konten

### Mode 5: Batch Judge (beberapa konten sekaligus)

1. User kirim 2+ konten → judge satu per satu
2. Ranking dari score tertinggi → terendah
3. Hanya APPROVED yang direkomendasikan

## Format Output — Threads

```
📊 **Judge Verdict: [✅ APPROVED / 🔄 NEEDS_REVISION / ❌ REJECTED]**
Overall: X.X/10

🎣 Hook:     X/10
💡 Clarity:   X/10
🔥 Engage:    X/10
✅ Accuracy:  X/10
✍️ Language:  X/10
📏 Format:    X/10

[Kalau NEEDS_REVISION atau REJECTED:]

⚠️ **Masalah:**
• [Masalah spesifik — bukan general]
• [Masalah spesifik]

💡 **Saran:**
• [Saran konkret & actionable]
• [Saran konkret]
```

**Untuk APPROVED:** Output ringkas — cukup verdict badge + overall score. Jangan breakdown panjang.

## Format Output — Dokumen/Ebook

```
📊 **Judge Verdict: [✅ APPROVED / 🔄 NEEDS_REVISION / ❌ REJECTED]**
Overall: X.X/10

📖 Content Quality:   X/10
🏗️ Structure & Flow:  X/10
✅ Accuracy:          X/10
✍️ Language & Tone:   X/10
📋 Completeness:      X/10
🎯 Targeting:         X/10
```

**Untuk DOKUMEN:** Breakdown WAJIB untuk SEMUA verdict (beda dengan Threads). Sertakan:
- Kelebihan per dimensi (spesifik, bukan pujian kosong)
- Masalah per dimensi (kalau ada)
- Saran revisi actionable
- Prediksi score kalau direvisi

## Kalibrasi Contoh

### Contoh APPROVED (8.2/10)

> 🧵 Google baru aja drop Gemini 2.5 dan ini BUKAN update kecil-kecilan.
>
> Yang bikin gila: reasoning ability-nya naik 40% dibanding versi sebelumnya. Mereka klaim ini model paling pintar yang pernah mereka buat — bahkan ngalahin GPT-4 di benchmark coding.
>
> #AI #GoogleGemini

| Dimensi | Score | Alasan |
|---------|-------|--------|
| Hook | 9 | "BUKAN update kecil-kecilan" = strong contrast hook |
| Clarity | 8 | Pesan jelas, satu big idea |
| Engagement | 9 | Data 40% + klaim "ngalahin GPT-4" = provoke reaction |
| Accuracy | 7 | Klaim "ngalahin GPT-4" perlu nuance, tapi generally true |
| Language | 9 | Natural, conversational, ada emphasis |
| Format | 7 | 2 post, char count OK, hashtag ok |
| **Avg** | **8.2** | ✅ APPROVED |

### Contoh NEEDS_REVISION (6.5/10)

> 🧵 AI makin canggih aja sekarang
>
> Banyak perusahaan yang ngembangin AI. Ada yang buat chatbot, ada yang buat coding. Semua berlomba-lomba bikin yang terbaik.
>
> Menurut gue ini menarik banget buat diikutin perkembangannya.
>
> #AI

| Dimensi | Score | Alasan |
|---------|-------|--------|
| Hook | 4 | Generik, gak ada hook spesifik |
| Clarity | 5 | Terlalu vague, gak jelas poinnya apa |
| Engagement | 3 | Gak ada yang memancing reply |
| Accuracy | 8 | Nothing factually wrong |
| Language | 7 | Natural tapi flat |
| Format | 8 | OK |
| **Avg** | **5.8** | ❌ REJECTED |

### Contoh REJECTED (too vague, no substance)

## Integrasi

### Dengan threads-content-from-link (Threads)

Setelah `threads-content-from-link` selesai generate:

```
Hasil generate → LLM Judge (auto) → APPROVED: kasih ke user
                                   → NEEDS_REVISION: kasih + saran
                                   → REJECTED: rewrite + judge ulang
```

## Pitfalls

### Threads-specific
- **Jangan terlalu strict** — konten Threads emang santai. Jangan expect formalitas kayak artikel jurnal.
- **Konteks audiens** — Arsy (user) target audiens tech/AI Indonesia. Sesuaikan ekspektasi.
- **Accuracy nuance** — klaim "paling pintar" / "terbaik" acceptable asalkan tidak blatantly false. Bedakan antara *opinion* dan *false claim*.
- **Jangan looping forever** — kalau setelah 2x rewrite masih REJECTED, stop dan tanya user.
- **Kalibrasi ulang kalau user ngasih feedback** — kalau user bilang "ini bagus kok" padahal lu REJECTED, berarti threshold lu terlalu tinggi. Kalau user bilang "ini gak bagus" padahal lu APPROVED, berarti lu terlalu longgar.
- **Output APPROVED jangan verbose** — user cuma perlu tau verdict + score. Simpan breakdown detail buat kasus NEEDS_REVISION / REJECTED aja.

### Dokumen/Ebook-specific
- **Deteksi dulu jenis konten** — jangan pake rubric Threads buat ebook. Kalau user bilang "Act as llm as judge, review ini" tanpa specify jenis, deteksi dari konten.
- **Mismatch struktural ≠ otomatis jelek** — kalau Daftar Isi gak match isi, bisa jadi artefak lama (draft). **Tanya user dulu** sebelum judge, jangan langsung vonis "bohong".
- **Threshold completeness lebih tinggi** — pembaca ebook ekspektasi konten utuh. Kalau mengklaim 6 bab tapi cuma ada 4, ini bukan "minor issue".
- **Cek akurasi klaim spesifik** — harga VPS ($5), kurs (80rb), command CLI (`hermes model`), nama fitur. Salah satu aja bisa nurunin trust.
- **Dokumen review SELALU breakdown lengkap** — beda sama Threads APPROVED yang cukup ringkas. Pembaca dokumen butuh detail.
