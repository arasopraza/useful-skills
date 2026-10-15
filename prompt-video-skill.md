---
name: prompt-video
description: "AI video prompt engineering: hyper-realistic formula, caption flows, Seedance templates (ASMR, B-roll, Image-to-Video). Covers camera specs, lighting physics, texture detail, negative prompts, and multi-option caption strategies."
tags: [video, prompt-engineering, ai-video, seedance, hyperrealistic, asmr]
---

# Prompt Video — AI Video Prompt Engineering

Kumpulan formula, template, dan strategi prompt untuk AI video generators (Sora, Runway, Kling, Seedance). Terdiri dari dua pendekatan utama yang saling melengkapi.

---

## Section 1: Hyper-Realistic Formula (Zero AI Flaw)

Formula untuk prompt video hyper-realistic + caption dengan spesifikasi kamera presisi, pencahayaan fisikal, dan anti-AI-flaw.

### Formula Prompt

```
Static locked-off [shot type], [resolution], [lokasi + setting detail], [waktu spesifik dengan elevasi matahari], [subjek + aksi detail dengan anchor point — cegah morphing], [detail tekstur spesifik], [detail atmosfer/cuaca], [spesifikasi kamera + lens], [depth of field], seamless looping dengan frame 1 identik final frame, [aspect ratio]
```

### Aturan Wajib

- **Waktu spesifik:** "4:47 PM", "5:52 AM civil dawn" — bukan "sore" atau "pagi" generik
- **Kamera + lens:** ARRI Alexa / Sony Venice / RED V-RAPTOR + lens spesifik (Master Anamorphic, Cooke S4, Zeiss Supreme Prime, dll)
- **Anchor point:** "every stalk anchored firmly to the same root point" — cegah floating/morphing
- **Pencahayaan:** temperatur Kelvin spesifik, elevasi matahari dalam derajat
- **Tekstur mikro:** "visible dew droplets", "cracked surface texture", "individual leaf venation" — bukan deskripsi generik

### Negative Prompt (Zero AI Flaw Template)

```
morphing, warping, texture swimming, texture shifting, ai sheen, plastic skin, glossy surfaces on organic matter, floating objects, sliding objects, physics violations, inconsistent shadow direction, shadow flicker, shadow popping, flickering, strobing, temporal instability, jitter, wobble, boiling artifacts, melting, shifting leaf count, deformation, water surface chaos, random splashing, reverse water flow, inconsistent wind direction, objects phasing through each other, disappearing objects, shape-shifting, liquid-metal textures, hyper-saturated HDR look, CGI render look, video game graphics, uncanny valley, double-exposure ghosting, frame blending artifacts, crossfade seams, inconsistent grain pattern, dead pixels, banding, compression macroblocking, lens flare, chromatic aberration, black bar edges, watermark, text overlay, camera movement, panning, zooming, tilting, drone wobble, handheld shake, non-loopable, start-end frame mismatch, frame discontinuity
```

### Caption Flow — 3 Opsi

Selalu kasih 3 opsi caption:

| Opsi | Fokus | Tone |
|------|-------|------|
| 1 | Psikologi + budaya | Reflektif, hangat |
| 2 | Hook viral + sains | Tajam, relate ke modern life |
| 3 | Filosofi budaya lokal | Puitis, kultural |

**Struktur Caption:**
- **Opsi 1:** Buka dengan "You can't look at [subject] without..." → jelasin fenomena psikologis (biophilia hypothesis, cortisol, dll) → tie-in ke budaya lokal → akhiri dengan ajakan sederhana
- **Opsi 2:** Buka dengan "There's a reason [subject] videos always..." → sains singkat (20 detik alam = kortisol turun) → kontras "leluhur vs kita" → CTA simpan
- **Opsi 3:** Buka dengan "In [culture], [subject] is more than..." → jelasin filosofi/istilah lokal (ngelmu titen, urip, wiwitan) → makna simbolis → ajakan kontemplatif

Panjang: 3-4 paragraf pendek. Gak pakai hashtag. Gak pakai numbering post (🧵 1/3).

### Checklist Sebelum Deliver

- [ ] Waktu spesifik + elevasi matahari?
- [ ] Kamera + lens spesifik?
- [ ] Anchor point disebut (anti-morphing)?
- [ ] Pencahayaan Kelvin?
- [ ] Tekstur mikro minimal 2?
- [ ] Negative prompt include semua istilah anti-AI-flaw?
- [ ] Caption 3 opsi sesuai struktur?
- [ ] Bahasa caption sesuai yang diminta (Indo/English)?

---

## Section 2: Seedance Template Library

Template siap pakai untuk Seedance AI — fokus pada **fixed camera / locked-off shot** untuk seamless looping (ASMR, ambience, B-roll estetik).

### Formula Dasar Seedance

```
[Shot type], [main subject + visual details], [one clear action], [environment + time of day], [camera movement], [lighting], [visual style], [aspect ratio]
```

Untuk **fixed camera / locked-off**, selalu cantumkan:
- `no camera movement, no panning, no zooming, no tilting`
- `completely locked-off camera`

Untuk **Image-to-Video**, buka dengan:
- `Animate this image with [gerakan], keep the character/scene consistent, no background change, [style]`

See `references/seedance-templates.md` for the full template library (ASMR Ocean Waves, Rainy Cafe, Cozy Reading, City Night).

### Panduan Pemakaian Seedance

**Text-to-Video:**
1. Copy compact prompt dari template
2. Paste langsung ke Seedance Text-to-Video
3. Tambahkan negative prompt
4. Gunakan hashtag untuk discoverability

**Image-to-Video:**
1. Upload reference image
2. Ubah prompt menjadi: `Animate this image with [detail gerakan], keep the scene/character consistent, no background change, [style]`
3. Negative prompt tetap sama, tambahkan `character drift, morphing, identity change, design inconsistency`

### Iterasi & Debugging

- Kalau hasil kurang pas, **ganti SATU variabel dulu** (gerakan / kamera / lighting), jangan rewrite semua
- Untuk looping: pastikan start dan end frame identik — Seedance lebih baik di T2V dengan deskripsi scene yang statis
- Fixed camera lebih stabil daripada camera movement — kurangi variabel gagal

### Template Karakter Animasi (Image-to-Video)

```
Animate this image with [karakter] [satu gerakan utama], [environment detail], [camera movement atau fixed], [lighting], [animation style], keep the character design and colors exactly as reference, [aspect ratio]
```

Contoh: `Animate this image with the ice cream character waving hello with one tiny hand and bouncing slightly, colorful candy playground background, slow push-in camera, bright warm sunlight, Pixar-style 3D animation, keep the character design and colors exactly as reference, 16:9`

---

## See Also

- `references/seedance-templates.md` — full 4-template library with compact prompts, negative prompts, and hashtags
- `prompt-video-hyperreal` (archived) — was the source for Section 1
- `prompt-video-template` (archived) — was the source for Section 2
