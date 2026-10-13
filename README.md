# APlatformIndicators — Presence Update Fix (Unofficial)

An **unofficial modification** of [Strencher's APlatformIndicators](https://github.com/Strencher/BetterDiscordStuff/blob/master/PlatformIndicators/APlatformIndicators.plugin.js) (source version 1.7.1). The original author is **Strencher**. This repository is not affiliated with or endorsed by the original author or BetterDiscord.

## Files

| File | Purpose |
| --- | --- |
| [`APlatformIndicators.Original.plugin.js`](APlatformIndicators.Original.plugin.js) | Original plugin logic, reconstructed from the supplied source (the pasted source had Markdown escaping/formatting) |
| [`APlatformIndicators.Fixed.plugin.js`](APlatformIndicators.Fixed.plugin.js) | Modified version with presence-store subscription |
| [`CHANGES.patch`](CHANGES.patch) | Exact line-by-line diff between the two included files |

## What changed?

The original `usePlatformStores` subscribes to `UserStore` and `SessionsStore` but reads other users' `PresenceStore` data without explicitly subscribing to that store. This can leave indicators stale until another UI update triggers a rerender.

The modified version:

1. Subscribes to `PresenceStore` through `Hooks.useStateFromStores`.
2. Uses a JSON string snapshot of the selected user's `clientStatuses` to detect updates, including in-place mutations.
3. Parses that subscribed snapshot when displaying another user's platforms.

The visual design, icon mapping, and other functionality are otherwise unchanged in the included modification.

## Important limitations

This is a **candidate stale-render fix**, not a guarantee of correct device detection. Discord's own presence data may be delayed or inaccurate. A mobile icon does **not** establish that a person picked up a phone, read a message, or deliberately ignored anyone. This modification has **not been verified in a live Discord client**; Discord internal APIs can change. The original file in this repository is reconstructed from the supplied code, not independently downloaded and authenticated from upstream.

## Installation

1. Download `APlatformIndicators.Fixed.plugin.js`.
2. Disable/remove any other copy of APlatformIndicators to avoid duplicate patches.
3. Move the fixed file into your BetterDiscord plugins folder.
4. Enable it in BetterDiscord and restart Discord.

## Testing

With a consenting test account, open and close Discord on mobile while watching its indicators in the DM list. Check whether indicators update without navigation or reloading. Repeat under idle/background conditions. This tests rendering behavior, not physical phone use.

## Attribution

Original plugin by [Strencher](https://github.com/Strencher/BetterDiscordStuff). Changes documented here are unofficial. Review upstream licensing and attribution requirements before redistributing.