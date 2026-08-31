# Ace's Super Cool Magic Converter Tool

Turn a saved play-by-post forum thread into a clean, readable PDF — no usernames,
no forum chrome, no quoted-back text. Just the story.

## What it does

Play-by-post threads are great to read while they're happening and miserable to
read afterward. The story is buried under signatures, avatars, OOC blocks, and
fifteen layers of quoted text. This pulls the actual prose out and lays it into a
PDF you can keep.

- Extracts post content and drops the surrounding forum furniture
- Preserves **bold**, *italic*, and text colors
- Strips quoted-back text by default (toggleable)
- Merges multiple pages of the same thread, sorted naturally
  (`page2` before `page10`, as it should be)
- Runs entirely in your browser — nothing is uploaded anywhere

## How to use it

1. Open the thread page you want to archive
2. **File → Save Page As → Webpage, HTML Only**
3. Repeat for each page of the thread
4. Open the tool, drag the saved files in, give it a title, hit build

The PDF downloads straight to your machine.

## Privacy

Everything happens client-side. Your saved pages are read by the browser, parsed
in memory, and turned into a PDF locally. No server, no upload, no analytics, no
network requests of any kind after the page loads. You can verify this by
disconnecting from the internet — the tool still works.

## Compatibility

Built against RPG Crossing, but the parser targets vBulletin-style post markup
generally, so other forums running similar software should work too. It looks for
`post_message_*` elements first, falls back to common `postbody` / `postcontent`
class patterns, and finally tries raw BBCode.

If a page comes back with zero posts extracted, that site's markup differs enough
that the selectors would need adjusting.

## Technical notes

Single self-contained HTML file. No build step, no dependencies to install, no
server. [jsPDF](https://github.com/parallax/jsPDF) is bundled inline rather than
loaded from a CDN, so the tool works offline, behind script blockers, and won't
break if a CDN goes down. Save the file to your desktop and it'll still run.

A few things that took more care than expected:

- **Character encoding.** These pages declare `charset=ISO-8859-1` but often
  contain Windows-1252 bytes. Reading them as UTF-8 turns every curly apostrophe
  into a replacement character. The tool sniffs the declared charset and decodes
  accordingly, then folds typographic punctuation down to ASCII, since jsPDF's
  built-in fonts are WinAnsi-only.
- **Rich text layout.** Word-wrapping across mixed bold/italic/colored runs is
  handled manually, measuring each word in its own font before deciding where the
  line breaks.

## Credits

- [jsPDF](https://github.com/parallax/jsPDF) — MIT License. Bundled inline; the
  license header is preserved in the source.
- Fonts: [Inter](https://fonts.google.com/specimen/Inter) and
  [Source Serif 4](https://fonts.google.com/specimen/Source+Serif+4), loaded from
  Google Fonts (the only external request the page makes; it falls back to system
  fonts if unavailable).

## Contact

Questions or flattering comments: PM **AceAffluence** on RPGX.
Unflattering comments: cash me outside.
