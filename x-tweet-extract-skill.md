---
name: x-tweet-extract
description: "Ekstrak konten tweet X/Twitter tanpa auth — pake syndication endpoint sebagai fallback kalau xurl gak ada"
version: 1.0.0
author: Arsy Opraza + Hermes
tags: [twitter, x, tweet-extraction, syndication, json]
---

# X/Twitter Tweet Extract

Ekstrak konten tweet tanpa perlu auth atau API key. Tiga metode berurutan dari yang paling reliable.

## Trigger

Gunakan skill ini ketika:
- Perlu ambil konten tweet untuk rewrite Threads
- xurl gak terinstall
- Browser kena login wall

## Workflow

### Step 1: Coba xurl dulu

```
xurl read <TWEET_ID>
```

Kalau sukses → skip ke Step 4.

### Step 2: Fallback 1 — Syndication endpoint (NO AUTH)

Endpoint ini gak perlu token apapun dan return full tweet JSON:

```bash
curl -s -L "https://cdn.syndication.twimg.com/tweet-result?id=<TWEET_ID>&token=0" | python3 -m json.tool
```

Fields penting:
- `text` — konten tweet lengkap
- `user.name` / `user.screen_name` — author
- `favorite_count` — likes
- `mediaDetails` — gambar/video

Kalau ini jalan → skip ke Step 4.

### Step 3: Fallback 2 — oEmbed API (terbatas)

```bash
curl -s "https://publish.twitter.com/oembed?url=https://x.com/<USERNAME>/status/<TWEET_ID>"
```

⚠️ Sering kosong/blocked. Hanya return HTML embed, bukan data terstruktur.

### Step 4: Parse & extract

Dari JSON response, ambil:
- `text` → konten tweet (bersihin dari t.co link kalau perlu)
- `created_at` → tanggal
- `entities.media` → ada gambar/video?
- `user.screen_name` → author handle

### Step 5: Beresin text

- Remove trailing t.co links kalau cuma media URL
- Untuk tweet thread, ambil satu per satu
- Perhatikan `display_text_range` buat tau panjang tweet asli

## Pitfalls

- Syndication endpoint kadang rate-limited. Kalau error, tunggu 5 detik retry.
- **X Articles (Twitter Notes) return metadata only** — syndication endpoint kasih title, preview_text, author, cover image tapi BUKAN full body article. Kalau user minta scrap X Article, langsung minta copy-paste konten atau screenshot. Jangan looping retry ke API/graphql.
- Tweet protected/deleted return empty.
- oEmbed hampir selalu useless — skip langsung ke syndication endpoint.
- Tweet ID diambil dari URL: `x.com/<user>/status/<TWEET_ID>`
