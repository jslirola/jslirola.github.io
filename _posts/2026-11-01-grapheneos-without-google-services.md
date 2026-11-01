---
layout: post
title: "GrapheneOS and apps without Google services (work in progress)"
date: 2026-11-01
---

> **Status: work in progress.** I am using this setup day to day and I will keep adding what I learn. Last updated: 2026-11-01. See the [changelog](#changelog) at the end.

This post is a starting point for someone who wants to stop depending on Google apps and services on their phone. I use [GrapheneOS](https://grapheneos.org) as the operating system, and I try to replace the apps I used to rely on with alternatives that do not need Google components.

It is not a complete guide, and it is not a security audit. It is what I did, what worked, and one problem that cost me my contacts for a while.

I use a Google Pixel 8. I installed GrapheneOS in the summer of 2026, on a phone I had just bought, and the installation went without any incidents. At the time of writing I am on GrapheneOS build 2026102100.

## What GrapheneOS is, and what it is not

GrapheneOS is an Android-based operating system. By default it ships without Google apps and services. It comes with its own browser, camera app, PDF viewer and app store, and it does not include the Google Play store.

Things to know before you decide:

- **It needs a supported device.** The installer only works on officially supported devices, and the GrapheneOS documentation recommends avoiding carrier variants, because they may not be unlockable. Check the official installation page for the current list instead of trusting any list in a blog post, including this one.
- **You can still use Google services if you need them.** GrapheneOS offers an optional, sandboxed version of Google Play that runs as an ordinary app, without the special privileges it has on stock Android. You decide whether to install it.
- **You control network access per app.** GrapheneOS lets you deny network access to an app, which standard Android does not. According to a review in LWN, network access is enabled by default for compatibility.

> **TODO verify:** check on grapheneos.org that these statements about sandboxed Google Play and network permissions match the current official documentation.

## Before you install

The official installation page recommends the **web installer** for most people. It runs in a browser with WebUSB support, and the page lists requirements such as a computer with at least 2 GB of free memory and 32 GB of free storage. Supported browsers include Chromium (outside Ubuntu's Snap package), Vanadium, Google Chrome and Microsoft Edge, and you should not use private browsing mode.

Two practical points:

- **Unlocking the bootloader wipes all the data on the phone.** Back up everything you want to keep first, including your contacts. This matters later in this post.
- **Follow the official guide step by step** instead of a summary from a third party: [grapheneos.org/install/web](https://grapheneos.org/install/web). I am not repeating the steps here on purpose.

## Getting apps without Google

Without the Play store, there are several places to get apps. These are the ones I know about, and I am not claiming any of them is safer than the others.

- **The GrapheneOS app store:** it mainly holds the project's own apps and what is needed for the sandboxed Google Play compatibility.
- **F-Droid:** a catalog of free and open source apps.
- **Obtainium:** it installs and updates apps directly from their release pages.
- **Aurora Store:** a client that lets you download apps from the Play catalog without signing in with a Google account.
- **Sandboxed Google Play:** only if an app you really need depends on it.

### What I use

I use **Obtainium** to install my apps and keep them updated. I also use **Aurora Store** for some apps, but I plan to remove it soon and get every app from a source tracked in Obtainium.

I do not use GrapheneOS user profiles for now, and I have not installed sandboxed Google Play.

### What I replaced

| Before | Now | Notes |
|---|---|---|
| YouTube | NewPipe | |
| Google Maps | Organic Maps | I do not consider it a good alternative yet |
| Telegram | Mercurygram | See the next section |

## Messaging: Mercurygram instead of Telegram

I used Telegram, and I wanted to stop using the official Android client because of its dependency on Google components.

[Mercurygram](https://github.com/Mercurygram/Mercurygram) is an unofficial, FOSS-friendly fork of the Telegram client for Android. According to its repository, it is maintained by rebasing its own patches, plus de-googling patches, on top of upstream Telegram. According to the project, it:

- removes the Google Play Services and Firebase dependencies,
- uses UnifiedPush instead of Google's push service for notifications,
- uses OpenStreetMap through MapLibre for location sharing instead of Google Maps.

You can install it from F-Droid, with Obtainium, from the GitHub releases, or from Google Play, according to the project.

Two things to keep clear:

- **It is still Telegram.** You use the same Telegram account and the same network. F-Droid's description of the app says that your messages and chats are stored in Telegram's cloud. Mercurygram removes Google components from the client, and it does not change how Telegram works behind it.
- **It is a fork maintained by a third party.** You are trusting its maintainer and its release process, not Telegram's official app. Read the project's page and decide if that is acceptable for you.

Notifications need a bit of setup. The project documents a menu to choose a UnifiedPush distributor, and it says that, for reliable notifications, you may need to set battery optimization to "Not optimized" for the app.

## A problem I had: "Sync Contacts" deleted contacts on my phone

This is the part I most want other people to read.

I turned on the **Sync Contacts** option in Mercurygram. Afterwards, the contacts in my phone's address book that did not have Telegram were gone from the device. What remained were the entries linked to Mercurygram: they showed the names of the users, but not their phone numbers, and I did not recognize any of them.

I had to restore a backup of my contacts to get my address book back, and I decided not to use Sync Contacts again.

What I can say for sure is what happened on my phone. I do not know the cause, and I have not checked whether it is expected behavior of the option or a bug.

If you are going to try it:

1. **Back up your contacts before you enable it.** Export them to a file and keep a copy off the phone.
2. **Enable it with care,** and check your address book right after.
3. **If you do not need Telegram to match your contacts, leave it off.** That is what I do now.

## What I will add next

This section stays open. I will update the post as I try more apps and settings.

## Changelog

- 2026-11-01: first version, with the Mercurygram section and the Sync Contacts problem.
- 2026-11-01: added my device, how I install apps, and the list of apps I replaced.

If you find something that needs correcting, open an issue in [the repository](https://github.com/jslirola/jslirola.github.io/issues).
