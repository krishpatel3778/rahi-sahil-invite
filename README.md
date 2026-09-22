# Rahi & Sahil — Ring Ceremony invite

Single-page invitation site. Static HTML, no build step, no dependencies,
no image files.

## Files

```
index.html     the page
invite.mp4     YOUR VIDEO — the only thing you add
```

That's it. No poster image needed.

## The video

Export 1080 × 1920, H.264, 30fps. Keep it under ~15 MB so it loads fast on
mobile data. Name it `invite.mp4` and put it next to `index.html`.

Because it loops, try to make the last frame resemble the first — the loop
point then reads as intentional rather than as a jump.

## Deploy

```bash
git init
git add .
git commit -m "Ring ceremony invitation"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/REPO-NAME.git
git push -u origin main
```

Then on GitHub: **Settings → Pages → Source: Deploy from a branch →
main / (root) → Save**. Live in a minute or two at
`https://YOUR-USERNAME.github.io/REPO-NAME/`.

The repo must be **public** for Pages to work on a free account.

## How the autoplay works

Every browser blocks autoplay with sound — there is no way around it, and
any tutorial claiming otherwise is wrong. So the page does what Instagram
and TikTok do: autoplay **muted** and loop, full-bleed, no play button.

## How people unmute

Four things work together, because a small icon on its own gets ignored:

1. **The whole video is the tap target.** Tapping anywhere unmutes — not
   just the pill. The buttons at the bottom still work normally.
2. **The first tap turns sound ON rather than toggling.** That is what
   someone tapping actually wants; toggling makes the first tap a coin flip.
3. **The label says "Tap for sound",** not "Sound". It names the action.
4. **A gold halo pulses out of the pill while muted,** and the equaliser
   bars move slowly. Motion in the corner draws the eye; the bars read as
   "there is audio here" faster than a speaker glyph does.

After about five seconds the pill shrinks to just the bars, so it stops
competing with the invitation. Turning sound on speeds up the bars, shows
"Sound on" for a moment, then tucks away again.

If autoplay is refused entirely (low-power mode, data saver), the label
changes to "Tap to play" and any tap starts it.

### Assume most people never unmute

Plenty of people open a forwarded link in public, or on a silenced phone,
and will watch the whole thing muted. Treat music as a bonus, never as
something the invitation depends on. Every word a guest must read —
names, date, time, venue — has to be legible on screen with the sound off.
The page handles its half of that; make sure the video does too.

## The loading state

There's no poster image. Instead the page shows a CSS-rendered card — the
names over a dark ground with gold light sweeping across it — which
cross-fades out the instant the first video frame decodes.

This is better than a poster file: nothing extra to export, nothing to
version, and it looks composed rather than like a placeholder. It also
means the page is never blank, even on a slow connection.

A 6-second timer forces the fade regardless, so a failed video load can
never leave someone stuck on the loading card.

## Before you send it

- [ ] Real start time — it's in **two** places: the `.time` paragraph and
      the `dates=` parameter in the calendar link
- [ ] Venue spelling confirmed (the road name especially)
- [ ] Opened on a real phone, not just a desktop browser
- [ ] Checked the loop point doesn't jar

## Optional: link previews

WhatsApp and Instagram DMs show a preview card for links. Without an
image they fall back to title and description text, which is fine. If you
want a picture there, export one frame as `preview.jpg`, put it in the
repo, and add this to the `<head>`:

```html
<meta property="og:image" content="https://YOUR-USERNAME.github.io/REPO-NAME/preview.jpg">
```

It has to be the full absolute URL — a relative path won't work.
