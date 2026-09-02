# Ace's Super Cool Magic Converter Tool

Turn a saved play-by-post forum thread into a clean, readable file — no usernames,
no forum chrome, no quoted-back text. Just the story.

**[Use it here →](https://YOUR-USERNAME.github.io/YOUR-REPO/)**

## What it does

Play-by-post threads are great to read while they're happening and miserable to
read afterward. The story is buried under signatures, avatars, OOC blocks, and
fifteen layers of quoted text. This pulls the actual prose out and gives you back
something you can actually use.

- Extracts post content and drops the surrounding forum furniture
- Preserves **bold** and *italic* (and text colors, in PDF)
- Strips quoted-back text by default (toggleable)
- Merges multiple pages of the same thread, sorted naturally
  (`page2` before `page10`, as it should be)
- Runs entirely in your browser — nothing is uploaded anywhere

## Output formats

| Format | Best for | Size (30 posts) |
|---|---|---|
| **Markdown** (default) | Feeding into an LLM, or any further processing | ~60 KB |
| **Plain text** | The simplest possible archive | ~59 KB |
| **PDF** | Reading and long-term archiving | ~1.6 MB |

Markdown is the default because it's the best format to hand to a language model.
A PDF stores text as positioned glyphs with no structure, so anything reading it
has to reconstruct where the words and paragraphs were — which is where errors
creep in. Markdown gives the model the actual characters plus explicit structure
(`**bold**`, `## Post 3`), at a fraction of the size and token count.

Use the PDF when a person is going to read it.

## How to use it

1. Open the thread page you want to archive
2. **File → Save Page As → Webpage, HTML Only** (or just Ctrl+S)
3. Repeat for each page of the thread
4. Open the tool, drag the saved files in, set a title, pick a format, hit build

The file downloads straight to your machine.

## Privacy

Everything happens client-side. Your saved pages are read by the browser, parsed
in memory, and converted locally. No server, no upload, no analytics, and no
network requests of any kind — the PDF library is bundled into the page itself
rather than loaded from a CDN. You can verify this by disconnecting from the
internet: the tool still works completely. Save the file to your desktop and it
runs there too.

## Compatibility

Built against RPG Crossing, but the parser targets vBulletin-style post markup
generally, so other forums running similar software should work too. It looks for
`post_message_*` elements first, falls back to common `postbody` / `postcontent`
class patterns, and finally tries raw BBCode.

If a page comes back with zero posts extracted, that site's markup differs enough
that the selectors would need adjusting.

## Technical notes

Single self-contained HTML file. No build step, no dependencies to install, no
server.

A few things that took more care than expected:

- **Character encoding.** These pages declare `charset=ISO-8859-1` but often
  contain Windows-1252 bytes. Reading them as UTF-8 turns every curly apostrophe
  into a replacement character. The tool sniffs the declared charset and decodes
  accordingly. The text formats keep that Unicode intact; only the PDF path folds
  typographic punctuation down to ASCII, because jsPDF's built-in fonts are
  WinAnsi-only.
- **Rich text layout in PDF.** Word-wrapping across mixed bold/italic/colored runs
  is handled manually, measuring each word in its own font before deciding where
  the line breaks.
- **Block boundaries.** Forum OOC blocks are built out of `<fieldset>` and
  `<legend>` elements, which had to be treated as block-level or the section
  label would run straight into the following text (`OOCMovement:`).

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
