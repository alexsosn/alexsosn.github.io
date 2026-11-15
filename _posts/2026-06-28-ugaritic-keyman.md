---
layout: post
title: "Typing Ugaritic on a Phone"
date: 2026-06-28 12:00:00 +0000
categories: linguistics tools
excerpt: A Keyman keyboard for Ugaritic transliteration, cuneiform signs, and editorial notation on iOS and Android.
comments: true
---

Typing Ugaritic on a phone is awkward. Most of the time I need Latin transliteration, but a regular keyboard has no convenient way to enter characters such as `ḫ`, `ṯ`, `ṣ`, `ʾ`, or `ʿ`. Copying them from a note or a character table works, but it gets tedious.

I made a [Keyman keyboard for Ugaritic](https://github.com/alexsosn/semitic-keyman) so I could type them directly on iOS and Android. It also includes Ugaritic cuneiform signs and some of the editorial symbols used in editions of ancient texts.

## The layout

Ugaritic was written in an alphabetic cuneiform script. In publications, the texts are commonly presented in Latin transliteration, which is easier to read and search but requires a fair number of characters that ordinary mobile keyboards leave out.

I wanted the keyboard to remain usable for normal typing. The main layer has familiar Latin letters, alongside dedicated keys for `ʔ`, `ʕ`, and `ø`. The less common characters live behind long presses. For instance, holding `h` opens variants used in transliteration, as well as a Ugaritic sign.

<img src="{{ '/images/ugaritic-keyman/abc-h-longpress.jpg' | relative_url }}" alt="Long-press menu for the h key, showing transliteration variants and Ugaritic signs" width="560" loading="lazy" />

*Long-pressing `h` on the main keyboard.*

The same approach works for vowels and other consonants. Where a corresponding cuneiform character is available, it appears alongside the Latin options. These are actual Unicode characters, so the result can be copied into a document or message like any other text (provided the font supports the Ugaritic script).

<img src="{{ '/images/ugaritic-keyman/abc-u-longpress.jpg' | relative_url }}" alt="Long-press options for u, including a Ugaritic cuneiform character and marked vowels" width="560" loading="lazy" />

*Long-pressing `u`: vowel variants and a cuneiform character.*

## Editorial marks and diacritics

Transcribing ancient texts involves more than choosing letters. You may need brackets for damaged or restored passages, or a combining mark that isn't available as a precomposed Unicode character.

The `123` layer contains punctuation, brackets, `√`, and the editorial signs `⸢` and `⸣`. The latter appear under more familiar-looking key labels, since not every font displays them correctly on a key.

<img src="{{ '/images/ugaritic-keyman/numeric-layout.jpg' | relative_url }}" alt="Numbers and editorial punctuation layer of the Ugaritic keyboard" width="560" loading="lazy" />

*The numbers and punctuation layer.*

There's also a `diac` layer for combining marks. The dotted circles on these keys are only visual guides; typing a key inserts the mark itself.

<img src="{{ '/images/ugaritic-keyman/diacritics-layout.jpg' | relative_url }}" alt="Combining diacritics layer with dotted-circle key labels" width="560" loading="lazy" />

*Combining marks on the `diac` layer.*

An optional suggestions package is available separately. It can suggest words in transliteration or cuneiform, but it isn't needed to use the keyboard.

<img src="{{ '/images/ugaritic-keyman/autocomplete-cuneiform-suggestions.jpg' | relative_url }}" alt="Optional word suggestions showing both Latin transliteration and Ugaritic cuneiform" width="340" loading="lazy" />

*Suggestions in transliteration and cuneiform.*

## Installing it

The keyboard uses [Keyman](https://keyman.com/), which supports custom keyboards on iOS and Android. Install the Keyman app, download [`ugaritic_translit.kmp`](https://github.com/alexsosn/semitic-keyman/blob/main/build/ugaritic_translit.kmp), and open the package in Keyman. You can also use **Settings → Install From File** in the app.

This is a touch layout for phones and tablets; it doesn't add a layout for a physical desktop keyboard.

The source is [on GitHub](https://github.com/alexsosn/semitic-keyman). I generate the Keyman files from Python scripts, with the transliteration-to-cuneiform mapping kept in one place, so changes to the layout are easier to maintain. The keyboard code is MIT-licensed.

If you use it and find a missing character or an inconvenient long-press choice, feel free to [open an issue](https://github.com/alexsosn/semitic-keyman/issues).
