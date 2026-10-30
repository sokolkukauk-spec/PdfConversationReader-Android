# PDF Conversation — native Android project

A small Java Android app that reads the dialogue already present in a PDF. It does **not** generate a podcast or summarise a paper into a conversation.

## Delivery status

Source project provided. An Android SDK, Gradle and Java compiler were not available in the authoring environment, so no APK was compiled and no emulator/device test was run. XML and package structure were checked. Treat this as a first implementation requiring a build and device validation, not a tested release.

## Features

- Android document picker: opens a PDF without broad storage permission.
- Extracts PDF text on a worker thread; lets you edit the result.
- Recognises explicit labels such as `Alex:` and `Sam:` at the start of a line.
- Assigns an installed offline TTS voice to each speaker.
- Play/pause, restart, previous/next passage and 0.5–2.0× speed.
- Displays the current speaker, text and passage number.
- Includes a sample conversation to test without a PDF.
- No server, API key, account, analytics or document upload.

## Build an installable APK — easiest route

1. Create a GitHub repository and upload **the contents of this folder** to its root, including `.github/workflows/build-apk.yml`.
2. Open its **Actions** tab. The included “Build Android APK” workflow runs on a push; it can also be started manually.
3. When it succeeds, open that run and download **PDF-Conversation-Android-APK** from Artifacts. Extract `app-debug.apk` from the downloaded ZIP.
4. Transfer the APK to your Android phone (Android 8.0 or newer), open it, and allow installation from the app you used to open it when Android asks.

The debug APK is for personal testing, not a Play Store release. The workflow has been supplied but has not been executed here. Do not upload private PDFs to the repository.

## Local build

Requirements: JDK 17, Gradle 8.7, Android SDK with platform 35 and its build tools. Android Gradle Plugin is pinned to 8.6.1. The project targets API 34 for this personal-testing build; store publication requirements need a separate review.

This source distribution does not contain a Gradle wrapper binary. Install Gradle 8.7, then run from this folder:

```sh
gradle wrapper --gradle-version 8.7
gradle testDebugUnitTest assembleDebug
```

Point `ANDROID_HOME` at your SDK or create `local.properties` containing `sdk.dir=/your/android/sdk`. You may then open the folder in Android Studio. Output: `app/build/outputs/apk/debug/app-debug.apk`. A network connection is needed for the first dependency download.

## Use

1. Install or download offline voices in Android text-to-speech settings.
2. Open PDF Conversation and try the sample.
3. Open your PDF. Review extracted text, especially columns, headers and speaker labels.
4. Tap **Apply text and set voices**. Select a voice for each speaker.
5. Press **Play**. Text edits are also applied automatically when playback starts.

Example:

```text
Alex: What is an AI agent?
Sam: A system that can choose actions and use tools to pursue a goal.
Alex: Why do permissions matter?
Sam: They limit the actions the agent can take.
```

Unlabelled opening text uses a Narrator voice. Wrapped lines continue the preceding speaker. A heading ending in a colon may be mistaken for a speaker: edit such headings before playing. For different languages, manually choose the appropriate voices.

## Limits

- Text-based PDFs only: no OCR and no password prompt. Supply an unlocked copy where permitted.
- Up to 20 MB, 250 pages and 500,000 extracted characters. Complex PDFs can still require significant memory; use smaller files if necessary.
- PDF layout is not displayed; this is a transcript reader. Column order and tables may extract imperfectly.
- Offline voice availability and quality depend on the installed Android speech engine. If only one voice is installed, speakers share it. The app filters for voices reported as offline-capable; speech is handled by the selected system engine.
- Pause replays the current short passage, rather than resuming at the exact word.
- Playback stops on leaving the app or locking the screen. No background service, audio export, OCR, saved document history or automatic conversation generation.
- Text and reading position are kept in memory only. Rotation is handled, but process termination loses the session.

## Validation before regular use

- Build successfully with the workflow; run its parser unit tests.
- On a physical phone: sample dialogue with two voices, pause/resume, next/previous, speed changes, text edits and restart.
- Import a text PDF, a scanned PDF, a protected PDF and an oversized file; verify appropriate messages and that the app stays responsive.
- Switch apps and lock the screen; verify speech stops. Return from speech settings and check voice availability.
- Try a multiline conversation, a PDF with columns and a non-English offline voice.

## Implementation references

Android TextToSpeech: https://developer.android.com/reference/android/speech/tts/TextToSpeech

PDFBox Android (Apache-2.0 dependency): https://github.com/TomRoush/PdfBox-Android

Dependencies are downloaded during the build; their source or binaries are not bundled in this archive.
