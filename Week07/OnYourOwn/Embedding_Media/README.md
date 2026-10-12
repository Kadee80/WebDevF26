# Embedding Video, Music & Maps

*On your own time · ~20 minutes to read, 5 minutes to use*

Nobody hosts their own video. A three-minute clip is a few hundred megabytes,
it needs to play on every device and connection, and GitHub Pages will not
serve it. What you do instead is **embed** — let YouTube, Vimeo or Spotify do
the hard part and show their player inside your page.

This is a reference, not a class. Read it when you need it.

---

## The `<iframe>`

Every embed on this page is an **`<iframe>`**: an element that loads *another
whole web page* inside a box on yours.

```html
<iframe src="https://example.com/thing"></iframe>
```

That is genuinely what is happening — YouTube's player is a small web page, and
your page is giving it a window. Which explains most of the quirks: your CSS
cannot reach inside it, their styles cannot leak out, and it loads separately
from everything else.

---

## YouTube

**Getting the code:** on the video, click **Share → Embed**, and copy what it
gives you. It looks like this:

```html
<iframe
  width="560"
  height="315"
  src="https://www.youtube.com/embed/dQw4w9WgXcQ"
  title="YouTube video player"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowfullscreen
></iframe>
```

**The mistake everybody makes once:** copying the address bar instead.

| | |
|---|---|
| ❌ `youtube.com/watch?v=dQw4w9WgXcQ` | the page you watch on. Will not embed |
| ✅ `youtube.com/embed/dQw4w9WgXcQ` | the player on its own. This one |

The code after `embed/` is the video id, and it is the same id in both URLs —
so you can convert by hand if you have to.

### Options worth knowing

Add them to the `src` as query parameters, `?` for the first and `&` after
that:

```html
src="https://www.youtube.com/embed/VIDEO_ID?start=42&rel=0&mute=1"
```

| Parameter | Does |
|---|---|
| `start=42` | begin 42 seconds in |
| `rel=0` | suggested videos at the end come from the same channel, not the whole of YouTube |
| `mute=1` | start muted |
| `autoplay=1` | start playing — **only works if also muted**. Browsers block noisy autoplay, and they are right to |
| `loop=1&playlist=VIDEO_ID` | loop it. The repeated id is not a typo; loop needs it |

### Two attributes worth adding yourself

```html
<iframe
  src="https://www.youtube-nocookie.com/embed/VIDEO_ID"
  loading="lazy"
  title="A short film about pigeons"
></iframe>
```

- **`youtube-nocookie.com`** is YouTube's privacy-friendly domain. Same player,
  fewer trackers set on your visitors.
- **`loading="lazy"`** means the embed does not load until the visitor scrolls
  near it. An embed is a whole extra page; three of them at the top of a slow
  page is a real problem.
- **`title`** is what a screen reader announces. An untitled iframe is read as
  "frame", which is useless. Always write one.

---

## Vimeo

Same idea, nicer-looking player, no suggested videos at the end — which is why
designers tend to prefer it.

**Share → Embed**, and you get:

```html
<iframe
  src="https://player.vimeo.com/video/76979871?h=8272103f6e"
  width="640"
  height="360"
  frameborder="0"
  allow="autoplay; fullscreen; picture-in-picture"
  allowfullscreen
></iframe>
```

The `h=` parameter is a privacy hash on newer videos — if you delete it the
embed may stop working, so keep the whole URL as given.

Useful parameters: `color=ff0000` (the player's accent colour), `title=0`,
`byline=0`, `portrait=0` to strip the uploader's details off the top.

---

## Spotify

**Right-click the track, album, playlist or episode → Share → Copy link**, then
turn the `/track/` URL into an `/embed/track/` one — or use Spotify's
[embed generator](https://developer.spotify.com/documentation/embeds), which
does it for you.

```html
<iframe
  src="https://open.spotify.com/embed/album/1DFixLWuPkv3KT3TnV35m3"
  width="100%"
  height="352"
  frameborder="0"
  allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture"
  loading="lazy"
></iframe>
```

Heights that work: **152px** for the compact player, **352px** for the one with
artwork, **80px** for the slim single-track bar.

Anyone can play a 30-second preview; people logged into Spotify get the whole
thing. You cannot control that, so do not design around it.

Works the same way for `/embed/track/`, `/embed/playlist/`,
`/embed/episode/` and `/embed/show/`.

---

## Google Maps

**Share → Embed a map**, copy, paste. Same `<iframe>` shape, and the same
advice about `loading="lazy"` and a real `title`.

---

## Making them responsive

Here is the problem. That embed code has `width="560" height="315"` baked into
it. On a phone, 560px is wider than the screen, and your page gets a horizontal
scrollbar.

Setting `width: 100%` in CSS fixes the width and breaks the shape — the height
stays at 315px and the video gets letterboxed or squashed.

**The modern fix is one property:**

```css
.video {
  width: 100%;
  aspect-ratio: 16 / 9;
}
```

```html
<iframe class="video" src="..." title="..."></iframe>
```

`aspect-ratio` tells the browser to work the height out from the width and keep
the proportion. Delete the `width` and `height` attributes from the HTML so
they do not fight your CSS.

Common ratios: **16 / 9** for most video, **4 / 3** for older footage,
**1 / 1** for square. Spotify is not a video and does not want a ratio — give
it `width: 100%` and leave the pixel height alone.

<details>
<summary>The old way, if you ever meet it in someone else's code</summary>

Before `aspect-ratio` existed, people did this:

```css
.video-wrapper {
  position: relative;
  padding-bottom: 56.25%;   /* 9 ÷ 16 = 0.5625 */
  height: 0;
  overflow: hidden;
}

.video-wrapper iframe {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
```

It works because **percentage padding is calculated from the parent's width**,
so `padding-bottom: 56.25%` reserves a box 16:9 regardless of screen size. A
genuinely clever hack, and completely unnecessary now. You only need to
recognise it.

</details>

---

## A drop-in snippet

Paste this into your midterm and change two things:

```html
<section class="media">
  <h2>Watch</h2>
  <iframe
    class="video"
    src="https://www.youtube-nocookie.com/embed/VIDEO_ID"
    title="Say what the video is here"
    frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowfullscreen
    loading="lazy"
  ></iframe>
</section>
```

```css
.media {
  max-width: 800px;
  margin: 0 auto;
}

.video {
  width: 100%;
  aspect-ratio: 16 / 9;
  border: 0;
  border-radius: 8px;
}
```

---

## Things that will catch you

- **The watch URL does not embed.** It has to be `/embed/`.
- **Nothing shows and the console says "refused to connect."** That site does
  not allow embedding, full stop. Nothing you can do from your side.
- **It works locally and not on GitHub Pages**, or the reverse. Some embeds
  refuse to load over `file://`. Test on the hosted version.
- **Your CSS does not work on the player.** Correct, and unfixable — it is
  another page. You can only style the box around it.
- **Autoplay with sound is blocked.** Every modern browser. Mute it or let the
  visitor press play.
- **An embed is a whole extra page to download.** Two or three of them is fine;
  ten will make your site crawl. `loading="lazy"` on all of them.
- **Every iframe needs a `title`.** It is one attribute and it is the
  difference between "frame" and "a short film about pigeons" for someone using
  a screen reader.

---

## For your midterm

Embeds are a cheap way to make a site feel finished, as long as the content is
*the point* rather than decoration. A record collection site with the albums
playable, a film studies project with the clips in it, a travel page with the
map — those work. A random video in a sidebar does not.

And check it on a phone. That is where the un-responsive ones fall over.
