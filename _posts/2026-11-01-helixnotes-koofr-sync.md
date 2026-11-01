---
layout: post
title: "Syncing HelixNotes between Windows and Android for free with Koofr (WebDAV)"
date: 2026-11-01
---

I wanted my notes in [HelixNotes](https://helixnotes.com) on my Windows PC and my Android phone to stay in sync. My requirements were simple: free, no server of my own to maintain, and no vendor lock-in for the notes themselves.

This post explains the setup I ended up with and why the obvious option (Proton Drive) did not work.

## How HelixNotes stores and syncs notes

HelixNotes keeps notes as plain Markdown (`.md`) files in a folder called a vault. Its built-in sync offers two providers: **Disabled** and **WebDAV**. The app describes WebDAV sync as meant for your own server (Nextcloud, ownCloud, a NAS).

Useful details from the Sync tab, which is the same on Windows and on Android:

- You provide a server URL, a username and an **app password** (not your main password). The password is stored locally on the device.
- There is a "Sync Now" button, plus automatic sync every 5, 15, 30 or 60 minutes, and optional sync when a note changes or when the vault opens.
- If a note is edited on two devices, both versions are kept and one is saved as a "(conflict)" copy.

## Why not Proton Drive?

Proton Drive was my first idea, but HelixNotes can only sync over WebDAV, and as of November 2026 Proton Drive does not offer native WebDAV support. Users have asked for it in Proton's feedback forum.

## Using Koofr as a free WebDAV server

[Koofr](https://koofr.eu) is a European cloud storage service (based in Slovenia) with a free plan of 10 GB that includes WebDAV access. For plain-text notes, that is far more than enough.

### 1. Create a folder and an app password

1. Create a folder in the Koofr web app. I named mine `Notes`.
2. Go to **Preferences > Password**, type a name and click **Generate**. Save the password right away: Koofr does not let you view it again.
3. Koofr's documentation recommends generating a separate app password for each connection. I follow that and use a new one for every device, for security. I have not tested whether reusing the same password on several devices works.

### 2. Configure HelixNotes

In **Settings > Sync**:

| Field | Value |
|---|---|
| Provider | WebDAV |
| Server URL | `https://app.koofr.net/dav/Koofr/Notes` |
| Username | The email of your Koofr account |
| Password | The app password generated for this device |

Koofr's WebDAV root is `https://app.koofr.net/dav/Koofr`. Files cannot be created at the very first level of `https://app.koofr.net/dav/`, so keep your notes inside a subfolder like `Notes`.

Press **Test Connection**, then **Sync Now**.

### 3. Syncing a second device

I configured the second device with the same server URL, its own app password and an empty vault. On the first sync, all the notes were downloaded. There were no conflicts, because nothing existed on that device yet.

I have not tested what happens if the second device already contains notes with the same names as the ones on the server.

### 4. Settings I chose

I enabled **Sync when a note changes** and **Sync when the vault opens**.

I tested propagating changes between the devices in every way I could think of, and I did not run into any problems.

## Privacy considerations

Everything in this section is based on what Koofr says about itself. I did not audit it, and I did not find an independent audit.

Koofr is based in the EU and states that it is GDPR compliant. As far as I could find, it does not use end-to-end encryption by default. It offers an optional client-side encryption feature called Koofr Vault, and I could not confirm that it works together with WebDAV sync from a third-party app. As with other cloud services that do not encrypt end to end by default, the provider could technically access your files, so consider that before storing sensitive notes.

## Limitations

- **Sharing notebooks between users:** I looked into using a shared Koofr folder as a vault for two people, and it is not something I would recommend. Koofr sharing permissions apply to a whole folder, and I could not confirm that a folder shared with you is writable through WebDAV.
- **Feature requests:** I searched the [HelixNotes issue tracker on GitLab](https://gitlab.com/ArkHost/HelixNotes/-/issues) in November 2026 and found no open request for an Android widget or for sharing notebooks between users. If you want either feature, that is the place to ask for it.

## Summary

- HelixNotes only syncs over WebDAV, so a service that offers WebDAV is required.
- Koofr's free plan provides it, plus 10 GB of storage.
- Use an app password per device, keep notes inside a subfolder, and start syncing a new device with an empty vault.

If you try this setup and find something that needs correcting, open an issue in [the repository](https://github.com/jslirola/jslirola.github.io/issues) and I will update the post.
