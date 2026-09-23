# Rahi & Sahil — Engagement invite

Single-page invitation. Static HTML, no build step, no dependencies.

```
index.html    the page
invite.mp4    the video (H.264, 1080×1920, faststart)
preview.jpg   1200×630 link-preview image (WhatsApp / iMessage / Instagram DMs)
```

## Flow

1. **Opener**: two gold rings draw themselves in and interlock, the stone drops into its setting, then sparkles, a light that runs around each band, and gold dust. It's all SVG + the Web Animations API, so it's vector-sharp like Lottie with no library and no JSON file.
2. **Tap**: the rings flare, gold particles burst out, an ivory bloom fades into the first frame of the video. The video plays **with sound**. The whole page goes fullscreen, not the `<video>` element (see below).
3. **Invitation**: when the video ends, or stops for **any** reason, the text animates in and the video shrinks to a card. The card says **Resume** when the video stopped partway and **Watch again** when it finished.

## Why it used to freeze

The page only moved to the invitation on the video's `ended` event. Anything else that stopped the video left it stuck in the "playing" state. In that state the text is hidden, the opener is gone and every tap target is switched off. The things that stop a video early:

- tapping pause or "Done" in the browser's native fullscreen player
- switching apps, locking the screen, a phone call, a Bluetooth headset disconnecting
- the network stalling

Fixes:

- any `pause` that isn't the natural end takes you to the invitation, with Resume
- the **scene** goes fullscreen instead of the video, so no native player takes over. On iPhone, where elements can't go fullscreen, the video plays inline edge to edge.
- a **Skip to invitation** pill appears while the video plays
- a buffering ring shows if loading takes more than 0.4 s
- text exits instantly. Before, it faded out on the same staggered delays it used to come in, so it hung over the growing video on replay.

## Other edge cases handled

- **Landscape phones / tiny screens**: when there's no room for the card, the text stacks, scrolls and gets a "Watch the video" button.
- **Web fonts load late**: the layout is measured again after `document.fonts.ready`.
- **Missing or broken video**: goes straight to the invitation with no empty card. This also covers a `<source>` 404 that happens before the script runs.
- **Double taps** are ignored, and `play()` being blocked falls back to the invitation.
- **Video cropping**: the video uses `object-fit: contain`, because `cover` cut off about 9% on each side of tall phones, which clipped the couple.
- **Slow first play**: `invite.mp4` was remuxed with `+faststart` (lossless). The index used to sit at the end of the file, so browsers had to fetch the tail before they could start playing.
- **Particle loop**: stops once the opener is gone and pauses in background tabs.
- **Reduced motion** gets static rings and instant transitions.

## Before you send it

- [ ] Hosts line: "Sudhir & Rajul Patel". Confirm the surname.
- [ ] Times. The video says "6:00 pm onwards". The page says Ceremony 6:00 / Dinner 7:30, and the calendar `dates=` runs 18:00–22:00.
- [ ] Test on a real iPhone **and** Android, opened from WhatsApp.

## Smaller video (optional)

It's about 15.6 MB at 8 Mbps. This roughly halves it with no visible loss on a phone:

```bash
ffmpeg -i invite.mp4 -c:v libx264 -crf 23 -preset slow -profile:v high -pix_fmt yuv420p \
       -c:a aac -b:a 128k -movflags +faststart invite-small.mp4
```

## Deploy

GitHub Pages → Settings → Pages → Deploy from branch `main` / root.
Live at `https://krishpatel3778.github.io/rahi-sahil-invite/`.
WhatsApp caches previews, so if you've already shared the link, test with `?v=2` added.
