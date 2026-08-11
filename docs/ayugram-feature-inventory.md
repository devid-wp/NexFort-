# AyuGram Feature Inventory

This inventory describes the AyuGram-specific surface currently included in the NexFort target. It is an audit record; it does not remove or disable any feature.

## Core Integration

- `ayu_worker`: background AyuGram worker lifecycle and session-related work.
- `ayu_settings`: AyuGram preferences persisted in `tdata/ayu_settings.json`.
- `ayu_state`: shared AyuGram runtime state.
- `ayu_lang`: AyuGram language integration.
- `ayu_infra`: core infrastructure and initialization hooks.
- `ayu_url_handlers`: AyuGram URL/deep-link handling.
- `ayu/utils`: Telegram mapping, helper operations, extended key modifiers, resource management, and Windows utilities.

## User-Facing Features

- `features/filters`: custom filters, filter controllers, cache handling, and filter utilities.
- `features/forward`: enhanced forwarding, rich forwarding text, and synchronization helpers.
- `features/message_shot`: message screenshots and screenshot theme state.
- `features/streamer_mode`: streamer mode with platform-specific implementations for Windows, macOS, and Linux.
- `features/translator`: translation provider abstraction, Google/Yandex providers, HTML parsing, and translation integration.

## Data and Persistence

- `data/ayu_database`: AyuGram SQLite database setup and access.
- `data/messages_storage`: local message persistence and retrieval.
- `data/entities`: AyuGram-specific stored entities.
- Bundled JSON and SQLite support under `ayu/libs/`.

## AyuGram UI

- `ui/settings`: AyuGram settings builder and settings sections for appearance, chats, filters, general options, and other options.
- `ui/context_menu`: AyuGram context menu extensions and item subtext.
- `ui/message_history`: custom history sections and message-history items.
- `ui/boxes`: theme selector, message shot, font, marks, import filters, donation, and plugin information boxes.
- `ui/components`: image view, icon picker, avatar-corner preview, message preview, and saved music components.
- `ui/utils`: palette, color processing, profile values, and iTunes search helpers.
- `ui/ayu_logo`, `ui/ayu_userpic`, `ui/toasts`: AyuGram branding and notification UI.
- `ui/*.style`: AyuGram icons and style definitions.

## Build Integration

- `Telegram/CMakeLists.txt` collects the AyuGram sources in `ayugram_files`.
- `ayugram::lib_icu` is linked for AyuGram filter and text-processing functionality.
- The inventory is intentionally broader than the current NexFort product decision. Each group must be classified before cleanup changes are attempted.

## Initial Classification

| Area | Decision | Rationale |
| --- | --- | --- |
| Core integration and utilities | KEEP | Required by the current AyuGram integration and shared feature lifecycle. |
| Filters | KEEP | A complete user-facing feature with settings, cache, and data dependencies. |
| Forwarding and synchronization | KEEP | User-facing power-user functionality with no confirmed removal decision. |
| Message shots | KEEP | User-facing utility with dedicated UI and theme state. |
| Streamer mode | KEEP | Privacy-oriented feature with platform-specific implementations. |
| Translator | KEEP | Existing user-facing feature; provider selection can be reviewed separately. |
| AyuGram data and persistence | KEEP | Required by filters and local message/data functionality. |
| AyuGram settings UI | MODIFY | Keep settings, but remove or rename AyuGram branding as part of NexFort branding. |
| AyuGram menus, history UI, boxes, and components | KEEP | Functional UI extensions remain useful; individual branding can be modified later. |
| AyuGram logo, userpic, icons, styles, and branded toasts | MODIFY | Branding must be aligned with NexFort while preserving functionality and notices. |
| Any unlisted or newly discovered feature | UNKNOWN | Requires a separate product decision before cleanup. |

No area is currently classified as `REMOVE`. Removal requires a confirmed product decision and a focused change with validation.
