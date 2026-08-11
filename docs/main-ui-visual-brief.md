# NexFort Main UI Visual Brief

## Direction

NexFort should feel like a focused desktop power tool: dense, calm, fast to scan, and unmistakably built for repeated chat workflows. Preserve Telegram Desktop's token-based style system and theme support. Avoid marketing composition, decorative cards, gradients, and ornamental panels.

## Desktop Layout

Use a three-region application shell:

1. **Navigation rail**
   - Width: existing compact window control scale.
   - Contains account/avatar, primary folders, saved items, settings, and command palette access.
   - Icon-first controls with tooltips; show labels only when the rail is expanded.
   - Active item uses the existing semantic active background and foreground tokens.

2. **Chat list column**
   - Fixed, resizable column between navigation and conversation content.
   - Header contains search, filter/folder access, and compose action.
   - Rows prioritize avatar, title, latest message preview, time, unread count, mute state, and draft state.
   - Keep row height stable across hover, unread, selected, and loading states.
   - Use one clear selected-row treatment; do not stack multiple competing badges.

3. **Conversation workspace**
   - Header contains peer identity, topic/context, search, call actions, and overflow actions.
   - Message history fills the primary vertical space.
   - Composer remains anchored to the bottom and expands only within a defined maximum height.
   - Secondary panels and previews slide from the right without changing the main column's identity.

## Responsive Behavior

- Below the compact desktop width, collapse the navigation rail into the chat-list header/menu.
- On narrow windows, show either chat list or conversation workspace as the primary view, with predictable back navigation.
- Never allow chat rows, toolbar buttons, or composer controls to resize because of text wrapping.
- Preserve keyboard focus visibility and the same active/selected semantics at every width.

## Visual Hierarchy

- Use one primary surface background, one elevated/over surface, one selected surface, and semantic status colors.
- Keep primary text high contrast, secondary text quieter, and metadata smaller without becoming faint.
- Reserve accent colors for active navigation, links, unread state, and explicit action feedback.
- Use existing day/night theme tokens instead of introducing raw colors in widgets.
- Keep border radius restrained and consistent with existing style tokens.

## Typography

- Reuse the shared normal and semibold font tokens.
- Conversation titles and chat names use semibold weight; previews and metadata use normal weight.
- Keep labels short and action-oriented.
- Do not use display-scale type inside the application shell.

## Interaction States

- Hover: subtle surface change, no layout shift.
- Active/selected: clear semantic background and foreground contrast.
- Focus: visible keyboard focus ring or equivalent existing control state.
- Pressed: use existing ripple/pressed token.
- Disabled: reduce contrast without removing legibility.
- Loading: preserve the final geometry and replace content with stable placeholders.

## Core Workflows

- Opening a chat should keep the selected chat row and conversation header visually synchronized.
- Search should be available from the chat-list header and conversation header without duplicating unrelated controls.
- Frequent actions should be reachable from keyboard shortcuts and the command palette.
- Context menus should expose actions in task order: open, reply/forward, save/copy, edit/delete, and advanced tools.
- Settings should use the same shell and spacing rhythm as the rest of the app.

## Implementation Constraints

- Put dimensions, spacing, colors, fonts, radii, and icon states in `.style` files.
- Keep C++ responsible for behavior and state, not visual constants.
- Reuse existing `dialogs/`, `history/`, `window/`, `settings/`, and `ui/` ownership boundaries.
- Make one workflow-sized change at a time and validate it before moving to the next region.
- Do not remove upstream functionality solely to achieve visual simplicity.

## First Slice

The first implementation slice should be the desktop shell and sidebar state only:

- define the navigation rail states;
- align chat-list and conversation selection state;
- verify compact-window collapse behavior;
- validate keyboard focus and existing theme tokens;
- leave message rendering and composer behavior unchanged until the shell is stable.

## Existing Ownership

- `window/window_controller.cpp::setupSideBar()` owns sidebar setup and reacts to filter-menu changes.
- `window/window.style` owns the sidebar width and `SideBarButton` visual variants.
- `window/window_filters_menu.*` owns filter/sidebar button creation, focus, and selection state.
- `mainwidget.*` owns the column geometry and the transition between dialogs, history, and secondary sections.

The first code change should extend these owners only after the desired sidebar state behavior is selected. No new parallel sidebar abstraction is needed.

## Chat List Ownership

- `dialogs/dialogs_widget.*` owns chat-list search, row selection, active-chat synchronization, narrow-layout behavior, and navigation into history.
- `dialogs/dialogs.style` owns dialog row palettes, text states, row dimensions, badges, and width-related tokens.
- `dialogs/dialogs_inner_widget.*` owns the scrollable row surface and keyboard/selection interactions inside the list.

The chat-list slice should first preserve row geometry and active selection while aligning the visual states with the brief. Search, badges, and row actions should remain behaviorally unchanged during that pass.

## Chat Header Ownership

- `history/view/history_view_top_bar_widget.*` owns the conversation header title, peer status, unread badge, search mode, call actions, info/menu actions, and narrow-layout controls.
- `history/history_widget.*` supplies the active-chat state and refreshes the header when the conversation changes.
- Header controls already have explicit search, selection, call, info, and menu objects; redesign should refine their grouping and visual priority without duplicating behavior.

The chat-header slice should preserve active-chat synchronization and search/call semantics while making the primary identity, secondary status, and overflow actions visually distinct.

## Message Bubble Ownership

- `history/view/history_view_message.*` owns message-view abstractions and message-specific rendering behavior.
- `history/view/history_view_list_widget.*` owns the visible message list and item layout delegation.
- `history/history_inner_widget.*` owns selection, repaint requests, scrolling, and interaction coordination.
- `ui/chat/chat.style` owns incoming/outgoing palettes, selected states, bubble radii, message typography, and message spacing tokens.

The message-bubble slice must preserve sender/media/service-message semantics and selection behavior. Visual changes should begin in semantic chat style tokens before touching message rendering code.
