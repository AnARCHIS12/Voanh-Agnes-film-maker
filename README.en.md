<div align="center">
  <img src="./assets/logo.svg" width="760" alt="CINÉ-GENESIS — AI filmmaking studio">
  <p>
    <img src="./assets/badges/status.svg" alt="Status: active">
    <img src="./assets/badges/version.svg" alt="Version: 1.0.0">
    <img src="./assets/badges/license.svg" alt="License: AGPL-3.0-or-later">
    <img src="./assets/badges/browser.svg" alt="Compatibility: modern browsers">
  </p>
  <p><a href="./README.md">Esperanto</a> · <a href="./README.fr.md">Français</a> · <strong>English</strong> · <a href="./README.es.md">Español</a></p>
</div>

# CINÉ-GENESIS

A standalone web application for creating AI-assisted short films, from the
initial idea to generated images and video shots.

## Features

| Feature | Description |
|---|---|
| Guided production | Seven structured steps from genre to final film |
| Visual continuity | Shared references across characters, scenes and shots |
| Video generation | Shot-by-shot production with automatic progress tracking |
| Work recovery | Automatic local saving after interruption |
| Multiple keys | Request distribution across several API keys |
| Privacy defaults | Tab-only keys by default and sanitized exports |
| Portable projects | Complete JSON project export |

## Required API

CINÉ-GENESIS works exclusively with the [Agnès AI](https://agnes-ai.com/) API.
Keys from OpenAI, Anthropic, Google Gemini or another provider will not work.

1. Open the official Agnès AI website.
2. Choose **Free API** or **Access API**.
3. Create or copy an API key from the developer area.
4. Paste it into the first CINÉ-GENESIS screen.

Plans, quotas, pricing and model names may change. Check the current Agnès terms
before starting a large generation job.

## Quickstart

One command clones and serves the application locally:

```bash
git clone https://github.com/AnARCHIS12/Voanh-Agnes-film-maker.git && cd Voanh-Agnes-film-maker && python3 -m http.server 8080
```

Then open `http://localhost:8080` in a modern browser.

## Creation workflow

1. **Connection** — enter Agnès keys and select the plan type.
2. **Format** — choose a genre and image ratio.
3. **Synopsis** — write freely or request an AI revision.
4. **Characters** — create profiles, costumes and reference portraits.
5. **Scenes** — define settings, mood, dialogue and key images.
6. **Shots** — generate the cinematic breakdown and initial frames.
7. **Videos** — generate, track and play the complete sequence.

Every result remains editable before moving to the next step.

## Technical configuration

| Purpose | Value |
|---|---|
| Main API | `https://apihub.agnes-ai.com/v1` |
| Video polling | `https://apihub.agnes-ai.com/agnesapi` |
| Authentication | `Authorization: Bearer YOUR_KEY` |
| Text | `agnes-3.0-flash`, then `agnes-2.5-flash` |
| Image | `agnes-image-2.5-flash` |
| Free video | `agnes-video-v2.0` |
| Token Plan video | `agnes-video-2.5-flash` |

The application calls `POST /v1/chat/completions`,
`POST /v1/images/generations`, `POST /v1/videos` and
`GET /agnesapi?video_id=...&model_name=...`.

## Architecture

The application has no dedicated backend. The interface, styles and logic live
in `index.html`. Projects are stored in `IndexedDB`, with `localStorage` as a
fallback.

```text
Project
├── settings, genre and synopsis
├── characters
│   └── profile and reference image
└── scenes
    ├── setting, mood, dialogue and image
    └── shots
        └── action, camera, line, image and video
```

```text
.
├── assets/
│   ├── badges/          Local SVG badges
│   └── logo.svg         Project visual identity
├── index.html           Complete application
├── LICENSE              AGPL-3.0-or-later
├── README.md            Esperanto
├── README.fr.md         French
├── README.en.md         English
└── README.es.md         Spanish
```

## Privacy and security

Projects stay in the browser, but AI operations are not local. Required text,
dialogue, descriptions, visual references and confirmed photos are sent to
Agnès AI. Generated videos may remain available through a remote URL.

- Keys remain in the current tab by default.
- Local persistence is optional and revocable.
- Keys are removed from JSON exports.
- Unencrypted and clearly local media URLs are rejected.
- A content security policy limits page capabilities.

Do not submit personal data, confidential work or third-party images without
the appropriate rights and informed consent.

## Compatibility

| Environment | Support | Notes |
|---|---|---|
| Chromium browsers | Full | Recommended for video and speech synthesis |
| Firefox | Full | Available voices depend on the operating system |
| Safari | Recent version | Check autoplay restrictions |
| Mobile browsers | Responsive interface | Keep the tab open during long generations |
| Offline | Partial | Editing works; generation requires the Agnès API |

## Contributing

1. Fork the repository and create a focused branch.
2. Keep the project free of external CDNs, telemetry and secrets.
3. Do not add emoji characters to the interface, documentation or commits.
4. Validate JavaScript syntax and responsive rendering.
5. Use a conventional commit such as `fix: protect exported API keys`.
6. Open a merge request with a clear description and verification results.

## License

This project is free software distributed under the
[GNU Affero General Public License v3.0 or later](./LICENSE). Modified versions
made available over a network must also offer their corresponding source code
under the license terms.
