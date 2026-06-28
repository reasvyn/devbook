# Video & Audio

## Description

The `<video>` and `<audio>` elements bring media to the web without requiring third-party plugins. Modern HTML5 media elements support multiple formats, accessibility features, captions, and programmatic control via JavaScript.

## Prerequisites

- [Elements, Tags & Attributes](elements-and-attributes.md) — fundamental HTML building blocks
- [Images & Responsive Images](images.md) — media considerations

## Table of Contents

- [The `<video>` Element](#the-video-element)
- [Video Formats and Codecs](#video-formats-and-codecs)
- [The `<audio>` Element](#the-audio-element)
- [Audio Formats](#audio-formats)
- [Source Element and Fallbacks](#source-element-and-fallbacks)
- [Media Attributes](#media-attributes)
- [Captions and Subtitles](#captions-and-subtitles)
- [Media Controls](#media-controls)
- [Programmatic Control](#programmatic-control)
- [Media Events](#media-events)
- [Accessibility](#accessibility)
- [Performance Considerations](#performance-considerations)
- [Common Mistakes](#common-mistakes)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The `<video>` Element

The `<video>` element embeds a video player in the page:

```html
<video src="presentation.mp4" width="640" height="360" controls>
    <p>Your browser does not support the video element.</p>
</video>
```

The text between the opening and closing tags is a fallback message shown in browsers that do not support HTML5 video.

**Required attributes:**
- `src` — video file URL

**Common attributes:**
- `controls` — show browser-native playback controls
- `width` / `height` — display dimensions (set both to prevent layout shift)
- `autoplay` — start playback automatically (blocked by many browsers without `muted`)
- `loop` — restart playback when finished
- `muted` — start with audio off
- `poster` — placeholder image shown before playback
- `preload` — hint for how much to preload (`none`, `metadata`, `auto`)

```html
<video src="intro.mp4" width="640" height="360" controls poster="thumbnail.jpg"
       preload="metadata" muted>
    <p>Your browser does not support video playback.</p>
</video>
```

### Video Formats and Codecs

Different browsers support different video formats. Providing multiple formats ensures broad compatibility.

| Format | Codec | Container | Support |
|---|---|---|---|
| MP4 | H.264 (AVC) | .mp4 | All browsers |
| WebM | VP8 or VP9 | .webm | Chrome, Firefox, Opera, Edge |
| Ogg | Theora | .ogv | Firefox, Chrome (deprecated) |
| HEVC | H.265 | .mp4 | Safari, some Edge |
| AV1 | AV1 | .mp4 or .webm | Chrome, Firefox (limited) |

**H.264 (MP4)** is the universal fallback — supported by every browser. Serve MP4 as the baseline and add WebM or AV1 as enhancements.

Provide multiple sources using the `<source>` element:

```html
<video width="640" height="360" controls>
    <source src="presentation.webm" type="video/webm">
    <source src="presentation.mp4" type="video/mp4">
    <p>Your browser does not support video playback.</p>
</video>
```

The browser selects the first source with a supported format. List formats from best compression to worst: AV1 → WebM → MP4.

### The `<audio>` Element

The `<audio>` element embeds an audio player:

```html
<audio src="podcast.mp3" controls>
    <p>Your browser does not support the audio element.</p>
</audio>
```

Attributes follow a similar pattern to video:

- `src` — audio file URL
- `controls` — show playback controls
- `autoplay` — start automatically (blocked without user interaction)
- `loop` — restart when finished
- `muted` — start with audio off
- `preload` — hint for preload behavior

### Audio Formats

| Format | Codec | Support |
|---|---|---|
| MP3 | MPEG-1 Audio Layer 3 | All browsers |
| AAC | Advanced Audio Coding | All browsers |
| Ogg Vorbis | Vorbis | Chrome, Firefox, Opera, Edge |
| FLAC | Free Lossless Audio Codec | Chrome, Firefox, Safari |
| WAV | PCM | All browsers |
| Opus | Opus | Chrome, Firefox, Safari 15+ |

MP3 is the universal fallback. Provide AAC as an alternative for better quality at the same bitrate:

```html
<audio controls>
    <source src="podcast.opus" type="audio/ogg; codecs=opus">
    <source src="podcast.mp3" type="audio/mpeg">
    <p>Your browser does not support audio playback.</p>
</audio>
```

### Source Element and Fallbacks

The `<source>` element provides multiple media sources for `<video>` and `<audio>`:

```html
<video width="640" height="360" controls>
    <source src="presentation.av1.mp4" type="video/mp4; codecs=av01.0.05M.08">
    <source src="presentation.webm" type="video/webm">
    <source src="presentation.mp4" type="video/mp4">
    <p>Your browser does not support video playback.
       <a href="presentation.mp4">Download the video</a>.</p>
</video>
```

Each `<source>` can have:
- `src` — media file URL
- `type` — MIME type with optional codecs parameter
- `media` — media query for responsive sources (video only)

The `media` attribute enables responsive video:

```html
<video controls>
    <source src="video-wide.mp4" type="video/mp4" media="(min-width: 800px)">
    <source src="video-square.mp4" type="video/mp4" media="(max-width: 799px)">
    <p>Your browser does not support video playback.</p>
</video>
```

### Media Attributes

**`controls`** — shows the browser's native controls (play/pause, volume, fullscreen, seek, timestamps):

```html
<video src="video.mp4" controls></video>
```

Without `controls`, there is no way for users to start playback. Either add `controls`, provide custom controls, or use `autoplay` (with caution).

**`autoplay`** — starts playback automatically:

```html
<video src="intro.mp4" autoplay muted></video>
```

Most browsers block autoplay with audio. The `muted` attribute enables autoplay without sound. Never autoplay videos with sound — it disorients users and violates accessibility guidelines.

**`loop`** — restarts playback when the end is reached:

```html
<video src="background.mp4" loop muted></video>
```

Useful for background videos, but ensure the content works in a loop (no narrative arc that requires a beginning and end).

**`muted`** — suppresses audio output:

```html
<video src="banner.mp4" autoplay muted loop></video>
```

The user can unmute via controls if they are enabled.

**`poster`** — display a placeholder image before playback:

```html
<video src="talk.mp4" controls poster="talk-thumbnail.jpg"></video>
```

The poster should represent the video content. Do not use misleading images.

**`preload`** — informs the browser how much to buffer before playback:

| Value | Behavior |
|---|---|
| `none` | Do not preload any content |
| `metadata` | Preload only metadata (duration, dimensions, poster) |
| `auto` | Preload as much as possible (default) |

```html
<video src="large-video.mp4" controls preload="none"></video>
```

Use `preload="none"` for videos below the fold to save bandwidth.

### Captions and Subtitles

The `<track>` element adds captions, subtitles, descriptions, chapters, and metadata to media elements:

```html
<video src="talk.mp4" controls>
    <track src="captions-en.vtt" kind="captions" srclang="en" label="English" default>
    <track src="captions-es.vtt" kind="captions" srclang="es" label="Spanish">
    <track src="subtitles-en.vtt" kind="subtitles" srclang="en" label="English Subtitles">
    <track src="chapters.vtt" kind="chapters" srclang="en" label="Chapters">
</video>
```

**`kind` values:**

| Kind | Purpose |
|---|---|
| `captions` | Dialogue + sound effects for hearing-impaired users |
| `subtitles` | Translated dialogue only |
| `descriptions` | Text description of visual content for blind users |
| `chapters` | Navigation markers for long content |
| `metadata` | Timed metadata for scripts |

WebVTT (Web Video Text Tracks) format:

```vtt
WEBVTT

1
00:00:01.000 --> 00:00:05.000
Welcome to this introduction to HTML.

2
00:00:06.000 --> 00:00:10.000
In this section, we'll cover the basics of
<v.loud>video and audio</v> elements.
```

WebVTT supports:
- Timestamps in `HH:MM:SS.mmm` format
- Basic formatting (`<b>`, `<i>`, `<u>`, `<c>`, `<v>`)
- Text alignment (`D:horizontal`, `D:vertical`)
- Cue settings for positioning

### Media Controls

The browser provides native controls when `controls` is present. These vary by browser:

**Chrome:** Play/pause, seek bar, volume slider, current/total time, fullscreen, picture-in-picture, settings (playback speed).

**Firefox:** Play/pause, seek bar, volume slider, current/total time, fullscreen, picture-in-picture.

**Safari:** Play/pause, seek bar, volume slider, current/total time, fullscreen, AirPlay.

**Custom controls** can be built with JavaScript and styled with CSS:

```html
<div class="video-player">
    <video id="myVideo" src="video.mp4"></video>
    <div class="controls">
        <button id="playBtn" aria-label="Play">▶</button>
        <input type="range" id="seekBar" min="0" max="100" value="0" aria-label="Seek">
        <span id="currentTime">0:00</span>
        <span>/</span>
        <span id="duration">0:00</span>
        <button id="muteBtn" aria-label="Mute">🔊</button>
        <button id="fullscreenBtn" aria-label="Fullscreen">⛶</button>
    </div>
</div>
```

Custom controls must be fully accessible: keyboard-operable, labeled with `aria-label`, and communicate state changes.

### Programmatic Control

The HTMLMediaElement interface provides JavaScript methods and properties:

**Playback control methods:**

```javascript
const video = document.getElementById('myVideo');

video.play();       // Start or resume playback (returns a Promise)
video.pause();      // Pause playback
video.load();       // Reload the media source
```

**Property access:**

```javascript
video.currentTime     // Current position in seconds (read/write)
video.duration        // Total duration in seconds (read-only, NaN before metadata loads)
video.volume          // Volume 0.0 to 1.0
video.muted           // Boolean for mute state
video.paused          // Boolean (read-only)
video.ended           // Boolean (read-only)
video.playbackRate    // Playback speed (1.0 = normal, 0.5 = half, 2.0 = double)
```

```javascript
// Skip ahead 30 seconds
video.currentTime += 30;

// Set volume to 50%
video.volume = 0.5;

// Play at double speed
video.playbackRate = 2.0;
```

**Checking support:**

```javascript
if (video.canPlayType('video/webm; codecs="vp9"')) {
    // Browser can play WebM VP9
}
```

`canPlayType` returns `"probably"`, `"maybe"`, or `""` (empty string for no support).

### Media Events

Media elements fire events at various stages of playback:

```javascript
const video = document.getElementById('myVideo');

video.addEventListener('loadstart', () => {
    console.log('Media loading started');
});

video.addEventListener('loadedmetadata', () => {
    console.log('Duration:', video.duration);
    console.log('Dimensions:', video.videoWidth, 'x', video.videoHeight);
});

video.addEventListener('loadeddata', () => {
    console.log('First frame of media loaded');
});

video.addEventListener('canplay', () => {
    console.log('Enough data to begin playback');
});

video.addEventListener('play', () => {
    console.log('Playback started');
});

video.addEventListener('pause', () => {
    console.log('Playback paused');
});

video.addEventListener('timeupdate', () => {
    console.log('Current time:', video.currentTime);
});

video.addEventListener('ended', () => {
    console.log('Playback complete');
});

video.addEventListener('error', () => {
    console.error('Media error:', video.error.message);
});
```

Common events for updating custom controls:

```javascript
// Update seek bar as video plays
video.addEventListener('timeupdate', () => {
    seekBar.value = (video.currentTime / video.duration) * 100;
    currentTimeDisplay.textContent = formatTime(video.currentTime);
});

// Update play/pause button
video.addEventListener('play', () => {
    playBtn.textContent = '⏸';
    playBtn.setAttribute('aria-label', 'Pause');
});

video.addEventListener('pause', () => {
    playBtn.textContent = '▶';
    playBtn.setAttribute('aria-label', 'Play');
});
```

### Accessibility

**Provide captions for all audio content.** Captions in WebVTT format benefit users who are deaf or hard of hearing, users in noisy environments, and users who prefer to read rather than listen.

**Provide transcripts for audio-only content:**

```html
<details>
    <summary>Transcript</summary>
    <p>Welcome to the podcast. In this episode, we discuss...</p>
</details>
```

**Do not autoplay videos with sound.** Autoplay with sound disorients users, especially screen reader users who may not know where the sound is coming from.

**Provide descriptive audio** for visually important video content. Use the `descriptions` track kind or include a separate audio description track.

**Ensure custom controls are keyboard accessible:**

```javascript
playBtn.addEventListener('keydown', (event) => {
    if (event.key === 'Enter' || event.key === ' ') {
        event.preventDefault();
        togglePlayback();
    }
});
```

**Announce state changes to screen readers:**

```javascript
function togglePlayback() {
    if (video.paused) {
        video.play();
        playBtn.setAttribute('aria-label', 'Pause');
    } else {
        video.pause();
        playBtn.setAttribute('aria-label', 'Play');
    }
}
```

**Use `aria-label` for all custom control buttons.** The native controls have built-in accessibility, but custom controls require explicit labeling.

### Performance Considerations

**Preload strategically:**
- Videos above the fold: `preload="metadata"` or `preload="auto"`
- Videos below the fold: `preload="none"` combined with lazy loading

**Use video compression:** Encode videos at appropriate bitrates for web delivery. A 1080p video compressed for web streaming is typically 5-10 Mbps (1-2 GB per hour).

**Provide multiple resolutions:** Serve lower-resolution videos on smaller screens to save bandwidth:

```html
<video controls>
    <source src="video-720p.mp4" type="video/mp4" media="(max-width: 1280px)">
    <source src="video-1080p.mp4" type="video/mp4" media="(min-width: 1281px)">
</video>
```

**Use adaptive streaming for long content:** For videos longer than a few minutes, consider HLS (HTTP Live Streaming) or DASH (Dynamic Adaptive Streaming over HTTP). These protocols split video into small segments and adjust quality based on network conditions. For simple embedding, serve a single well-compressed file.

**Consider bandwidth costs:** Video is expensive to serve. Use a CDN, optimize compression, and avoid autoplay for large files.

**Do not load videos on page load unless the user is likely to play them.** Use `preload="none"` for most embedded videos.

### Common Mistakes

**Missing captions.** Videos without captions exclude deaf and hard-of-hearing users and violate WCAG compliance requirements.

**Autoplay with sound.** Most browsers block it anyway, and it creates a poor user experience.

**No poster image.** Without a poster, the video player shows a black rectangle before playback.

**Not specifying width and height.** Without dimensions, the video player has zero height until the metadata loads, causing layout shifts.

**Single format.** Providing only MP4 works in all browsers, but adding WebM or AV1 saves bandwidth for users whose browsers support them.

**Custom controls without accessibility.** Custom buttons need keyboard handlers, ARIA labels, and state announcements.

**Forgetting the `muted` attribute on background videos.** Background videos that autoplay should always be muted.

**Not handling the play() promise rejection.** `video.play()` returns a promise that rejects if autoplay is blocked:

```javascript
video.play().catch(error => {
    // Autoplay was blocked — update UI to show a play button
    showPlayButton();
});
```

## Study Cases

**Case 1: The video that never loads on mobile.**

A developer embeds a 4K video without providing lower-resolution sources. On a mobile connection, the video takes 30+ seconds to buffer and drains the user's data plan.

The fix: provide multiple resolutions using the `media` attribute on `<source>`:

```html
<video controls>
    <source src="video-4k.mp4" type="video/mp4" media="(min-width: 1921px)">
    <source src="video-1080p.mp4" type="video/mp4" media="(min-width: 769px)">
    <source src="video-480p.mp4" type="video/mp4" media="(max-width: 768px)">
    <p>Your browser does not support video.</p>
</video>
```

**Case 2: The inaccessible media player.**

A developer builds custom controls using `<div>` elements without keyboard handlers, ARIA labels, or focus management. Keyboard users cannot operate the player, and screen reader users hear unlabeled buttons.

The fix: use native controls (`controls` attribute) unless custom controls are strictly necessary. If building custom controls, ensure every button is keyboard-accessible, labeled with `aria-label`, and communicates state to screen readers.

**Case 3: The background video that plays sound.**

A developer adds an autoplay background video without the `muted` attribute. On desktop Chrome, the video is blocked. On mobile, the video does not play at all. On Firefox, it plays with sound.

The fix:

```html
<video src="background.mp4" autoplay muted loop playsinline poster="bg-still.jpg"></video>
```

`playsinline` is required for iOS Safari to allow inline playback.

## Examples

**Example 1: Accessible video with multiple formats and captions.**

```html
<video id="tutorial" width="800" height="450" controls preload="metadata"
       poster="tutorial-thumb.jpg">
    <source src="tutorial.av1.mp4" type="video/mp4; codecs=av01.0.05M.08">
    <source src="tutorial.webm" type="video/webm">
    <source src="tutorial.mp4" type="video/mp4">
    <track src="captions-en.vtt" kind="captions" srclang="en" label="English" default>
    <track src="captions-es.vtt" kind="captions" srclang="es" label="Spanish">
    <track src="chapters.vtt" kind="chapters" srclang="en" label="Chapters">
    <p>Your browser does not support video.
       <a href="tutorial.mp4">Download the video</a>.</p>
</video>
```

**Example 2: Audio player with transcript.**

```html
<figure>
    <figcaption>Episode 42: The Future of Web Development</figcaption>
    <audio controls preload="metadata">
        <source src="podcast-ep42.mp3" type="audio/mpeg">
        <source src="podcast-ep42.opus" type="audio/ogg; codecs=opus">
        <p>Your browser does not support audio.
           <a href="podcast-ep42.mp3">Download the episode</a>.</p>
    </audio>

    <details>
        <summary>Transcript</summary>
        <div class="transcript">
            <p><strong>[0:00]</strong> Host: Welcome to DevBook FM. Today we're discussing
            the future of web development with our guest...</p>
        </div>
    </details>
</figure>
```

**Example 3: Custom video player controls.**

```html
<div class="custom-player">
    <video id="customVideo" src="demo.mp4" preload="metadata"></video>

    <div class="custom-controls" role="toolbar" aria-label="Video controls">
        <button id="customPlay" aria-label="Play" class="control-btn">▶</button>
        <input type="range" id="customSeek" class="seek-bar" min="0" max="100" value="0"
               aria-label="Seek position">
        <span id="customTime" class="time-display">0:00 / 0:00</span>
        <button id="customMute" aria-label="Mute" class="control-btn">🔊</button>
        <input type="range" id="customVolume" class="volume-bar" min="0" max="1" step="0.05"
               value="1" aria-label="Volume">
        <button id="customFullscreen" aria-label="Fullscreen" class="control-btn">⛶</button>
    </div>
</div>

<script>
    const video = document.getElementById('customVideo');
    const playBtn = document.getElementById('customPlay');
    const seekBar = document.getElementById('customSeek');
    const timeDisplay = document.getElementById('customTime');
    const muteBtn = document.getElementById('customMute');
    const volumeBar = document.getElementById('customVolume');
    const fullscreenBtn = document.getElementById('customFullscreen');

    function formatTime(seconds) {
        const m = Math.floor(seconds / 60);
        const s = Math.floor(seconds % 60).toString().padStart(2, '0');
        return `${m}:${s}`;
    }

    playBtn.addEventListener('click', () => {
        if (video.paused) {
            video.play();
        } else {
            video.pause();
        }
    });

    video.addEventListener('play', () => {
        playBtn.textContent = '⏸';
        playBtn.setAttribute('aria-label', 'Pause');
    });

    video.addEventListener('pause', () => {
        playBtn.textContent = '▶';
        playBtn.setAttribute('aria-label', 'Play');
    });

    video.addEventListener('timeupdate', () => {
        const progress = (video.currentTime / video.duration) * 100;
        seekBar.value = progress;
        timeDisplay.textContent = `${formatTime(video.currentTime)} / ${formatTime(video.duration)}`;
    });

    seekBar.addEventListener('input', () => {
        video.currentTime = (seekBar.value / 100) * video.duration;
    });

    muteBtn.addEventListener('click', () => {
        video.muted = !video.muted;
        muteBtn.textContent = video.muted ? '🔇' : '🔊';
        muteBtn.setAttribute('aria-label', video.muted ? 'Unmute' : 'Mute');
    });

    volumeBar.addEventListener('input', () => {
        video.volume = volumeBar.value;
        video.muted = false;
        muteBtn.textContent = '🔊';
    });

    fullscreenBtn.addEventListener('click', () => {
        if (document.fullscreenElement) {
            document.exitFullscreen();
        } else {
            document.querySelector('.custom-player').requestFullscreen();
        }
    });

    video.play().catch(() => {
        // Autoplay blocked — show overlay play button
        playBtn.textContent = '▶';
    });
</script>
```

**Example 4: Responsive video with art direction.**

```html
<video controls poster="demo-wide.jpg">
    <source src="demo-wide.webm" type="video/webm" media="(min-width: 800px)">
    <source src="demo-wide.mp4" type="video/mp4" media="(min-width: 800px)">
    <source src="demo-square.webm" type="video/webm" media="(max-width: 799px)">
    <source src="demo-square.mp4" type="video/mp4" media="(max-width: 799px)">
    <p>Your browser does not support video.</p>
</video>
```

## Glossary

| Term | Definition |
|------|------------|
| `<video>` | Embeds video content in a page |
| `<audio>` | Embeds audio content in a page |
| `<source>` | Provides multiple media sources for video/audio |
| `<track>` | Adds timed text tracks (captions, subtitles, chapters) |
| WebVTT | Web Video Text Tracks format for captions |
| Codec | Algorithm for encoding/decoding media data |
| Container | File format wrapping encoded media (MP4, WebM) |
| H.264 | Widely supported video codec (AVC) |
| VP9 | Royalty-free video codec by Google |
| AV1 | Royalty-free next-gen video codec |
| MP3 | Widely supported audio codec |
| AAC | Advanced Audio Codec |
| `autoplay` | Attribute that starts playback automatically |
| `controls` | Shows browser-native playback controls |
| `muted` | Suppresses audio output |
| `poster` | Placeholder image before playback |
| `preload` | Hint for how much media to buffer |
| `canPlayType()` | Method to check format support |
| `currentTime` | Current playback position in seconds |
| `playbackRate` | Playback speed multiplier |
| `playsinline` | iOS attribute for inline video playback |
| HLS | HTTP Live Streaming — adaptive streaming protocol |
| DASH | Dynamic Adaptive Streaming over HTTP |
| HTMLMediaElement | JavaScript interface for media elements |

## Quick References

- [MDN: Video Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video) — complete reference
- [MDN: Audio Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/audio) — complete reference
- [MDN: Track Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/track) — captions reference
- [MDN: HTMLMediaElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement) — JavaScript API
- [MDN: Media Events](https://developer.mozilla.org/en-US/docs/Web/Guide/Events/Media_events) — event reference
- [MDN: WebVTT](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API) — caption format reference
- [HTML Spec: Media Elements](https://html.spec.whatwg.org/multipage/media.html) — official specification
- [Mozilla: Video for Everybody](https://robinwhittleton.com/video-for-everybody/) — fallback strategy

## Next Steps

- [Canvas Graphics](canvas-graphics.md) — 2D drawing with JavaScript
- [SVG in HTML](svg-and-html.md) — scalable vector graphics
- [IFrames & Embeds](iframes-and-embeds.md) — embedding external content
