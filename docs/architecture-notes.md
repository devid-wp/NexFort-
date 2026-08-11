# NexFort Architecture Notes

## Project Map

NexFort is based on AyuGram Desktop and Telegram Desktop. The application is built from shared Desktop App libraries, Telegram-specific modules, and AyuGram-specific extensions.

### Main UI

- `Telegram/SourceFiles/ui/` contains shared widgets, controls, boxes, chat UI helpers, and style definitions.
- `Telegram/SourceFiles/dialogs/` contains the chat list and dialog selection UI.
- `Telegram/SourceFiles/history/` contains message history models, rendering, media views, and compose controls.
- `Telegram/SourceFiles/settings/` contains settings builders and settings sections.
- `Telegram/SourceFiles/window/` owns window sections, navigation, chat switching, and session-level presentation.
- `Telegram/cmake/lib_ui/` contains the shared UI library implementation.

### State and Storage

- `Telegram/SourceFiles/core/core_settings.*` stores application-level settings.
- `Telegram/SourceFiles/main/main_session_settings.*` stores session-level settings and serialized session state.
- `Telegram/SourceFiles/storage/` provides local storage, serialization, account/domain files, file locks, downloads, and cache integration.
- `Telegram/SourceFiles/ayu/data/` provides AyuGram-specific data and SQLite-backed message storage.

### Messages and Navigation

- `Telegram/SourceFiles/api/` and `apiwrap.*` issue Telegram API requests and process server updates.
- `Telegram/SourceFiles/data/` maps API data into application models.
- `Telegram/SourceFiles/history/` displays and mutates message history.
- `Telegram/SourceFiles/chat_helpers/` handles compose, autocomplete, stickers, emoji, and related chat interactions.
- `Telegram/SourceFiles/window/window_session_controller.*` exposes navigation operations such as opening peer history and peer information.
- `Telegram/SourceFiles/mainwidget.*` coordinates visible sections and history presentation.
- `Telegram/SourceFiles/window/window_chat_switch_process.*` handles chat switching transitions.

### Hotkeys

- `Telegram/SourceFiles/core/shortcuts.*` registers commands, default shortcuts, custom shortcut JSON, and action dispatch.
- `Telegram/SourceFiles/core/application.*` starts and finishes the global shortcut system.
- Context-specific `keyPressEvent` handlers live in UI and window components.

### Network and Updates

- `Telegram/SourceFiles/mtproto/` implements transport, connections, data-center configuration, authorization keys, sessions, and request sending.
- `Telegram/SourceFiles/api/` contains feature-level API clients and update handlers.
- `Telegram/SourceFiles/core/update_checker.*` checks and downloads application updates.
- `Telegram/SourceFiles/_other/updater_*` contains platform-specific update installation helpers.

### AyuGram Extensions

- `Telegram/SourceFiles/ayu/ayu_*.{h,cpp}` contains AyuGram core state, settings, language, infrastructure, workers, and URL handling.
- `Telegram/SourceFiles/ayu/features/` contains filters, forwarding, message shots, streamer mode, and translation.
- `Telegram/SourceFiles/ayu/ui/` contains AyuGram settings, menus, message-history UI, components, boxes, icons, and styles.
- `Telegram/SourceFiles/ayu/data/` contains AyuGram entities, database access, and message storage.
- `Telegram/CMakeLists.txt` collects these sources in the `ayugram_files` target source list and links AyuGram ICU support.

## Major Dependencies

### Required for the base client

- Qt: application framework, GUI, networking, multimedia integration, and platform abstractions.
- Desktop App libraries: `lib_base`, `lib_crl`, `lib_ui`, `lib_tl`, `lib_storage`, `lib_lottie`, `lib_qr`, `lib_translate`, `lib_webrtc`, `lib_webview`, and `lib_spellcheck`.
- Telegram Desktop libraries: `td_mtproto`, `td_scheme`, `td_lang`, `td_ui`, `td_export`, `td_iv`, `td_tde2e`, and `td_webauthn`.
- OpenSSL for cryptographic operations and MTProto security.
- Storage and serialization components for accounts, sessions, caches, and local files.

These components are part of the Telegram target's normal link graph and are required for a functional base client, even when some individual features are not used.

### Required by optional product features

- FFmpeg for media decoding and frame processing.
- WebRTC and tgcalls for real-time audio/video calls and related networking.
- FIDO2/libfido2 and libcbor for passkeys/security-key support.
- Prisma for platform or security integration used by the Telegram build.
- Stripe for payment-related features.
- OpenAL for audio output.
- QR code generator for QR UI and authentication flows.
- Spellcheck, WebView, translation, auto-update, and crash-report components for their corresponding product features.

### Rendering, compression, and utilities

- rlottie for animated vector graphics.
- zlib, minizip, lz4, and xxHash for compression, archives, and hashing.
- KCoreAddons and platform-specific Qt integrations where enabled.

### AyuGram-specific dependencies

- AyuGram ICU integration (`ayugram::lib_icu`) for filter and text-processing features.
- Bundled SQLite ORM and SQLite sources under `Telegram/SourceFiles/ayu/libs/` for AyuGram data storage.
- Bundled JSON libraries under `Telegram/SourceFiles/ayu/libs/` for AyuGram settings and data exchange.

These three dependencies are required by the currently included AyuGram features. Removing one would require removing or rewriting the feature that includes it.

### Bundled third-party components

The repository keeps additional source or integration directories under `Telegram/ThirdParty/`, including `ffmpeg`, `tgcalls`, `libfido2`, `libcbor`, `libprisma`, `rlottie`, `QR`, `kcoreaddons`, `xxHash`, `lz4`, `hunspell`, `cmark-gfm`, `range-v3`, `GSL`, and platform input-method integrations. Their necessity, licenses, and maintenance status require separate review.

## Audit Status

This document records architecture, the major-dependency inventory, and the current necessity classification. License review and stale-dependency review are intentionally separate tasks.
