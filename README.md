# unidic-py

This is a version of [UniDic](https://ccd.ninjal.ac.jp/unidic/) for
Contemporary Written Japanese packaged for use with pip.

Currently it supports 3.1.0, the latest version of UniDic. **Note this will
take up 770MB on disk after install.** If you want a small package, try
[unidic-lite](https://github.com/polm/unidic-lite).

The data for this dictionary is hosted as part of the AWS Open Data
Sponsorship Program. You can read the announcement
[here](https://aws.amazon.com/jp/blogs/news/published-unidic-mecab-on-aws-open-data/).

After installing via pip, you need to download the dictionary using the
following command:

    python -m unidic download

With [fugashi](https://github.com/polm/fugashi) or
[mecab-python3](https://github.com/samurait/mecab-python3) unidic will be used
automatically when installed, though if you want you can manually pass the
MeCab arguments:

    import fugashi
    import unidic
    tagger = fugashi.Tagger('-d "{}"'.format(unidic.DICDIR))
    # that's it!

## Differences from the Official UniDic Release

This has a few changes from the official UniDic release to make it easier to use.

- entries for 令和 have been added
- single-character numeric and alphabetic words have been deleted
- `unk.def` has been modified so unknown punctuation won't be marked as a noun

See the `extras` directory for details on how to replicate the build process.

## Fields

Here is a list of fields included in this edition of UniDic. For more information see the [UniDic FAQ](https://unidic.ninjal.ac.jp/faq#col_name), though not all fields are included. For fields in the UniDic FAQ the name given there is included.

Fields which are not applicable are usually marked with an asterisk (`*`).

- **pos1, pos2, pos3, pos4**: Part of speech fields. The earlier fields are more general, the later fields are more specific.
- **cType:** 活用型, conjugation type. Will have a value like `五段-ラ行`.
- **cForm:** 活用形, conjugation shape. Will have a value like `連用形-促音便`.
- **lForm:** 語彙素読み, lemma reading. The reading of the lemma in katakana, this uses the same format as the `kana` field, not `pron`.
- **lemma:** 語彙素（＋語彙素細分類）. The lemma is a non-inflected "dictionary form" of a word. UniDic lemmas sometimes include extra info or have unusual forms, like using katakana for some place names.
- **orth:** 書字形出現形, the word as it appears in text, this appears to be identical to the surface.
- **pron:** 発音形出現形, pronunciation. This is similar to kana except that long vowels are indicated with a ー, so 講師 is こーし.
- **orthBase:** 書字形基本形, the uninflected form of the word using its current written form. For example, for 彷徨った the lemma is さ迷う but the orthBase is 彷徨う.
- **pronBase:** 発音形基本形, the pronunciation of the base form. Like `pron` for the `lemma` or `orthBase`.
- **goshu:** 語種, word type. Etymological category. In order of frequency, 和, 固, 漢, 外, 混, 記号, 不明. Defined for all dictionary words, blank for unks.
- **iType:** 語頭変化化型, "i" is for "initial". This is the type of initial transformation the word undergoes when combining, for example 兵 is へ半濁 because it can be read as べい in combination. This is available for <2% of entries.
- **iForm:** 語頭変化形, this is the initial form of the word in context, such as 基本形 or 半濁音形.
- **fType:** 語末変化化型, "f" is for "final", but otherwise as iType. For example 医学 is ク促 because it can change to いがっ (apparently). This is available for <0.1% of entries.
- **fForm:** 語末変化形, as iForm but for final transformations.
- **iConType:** 語頭変化結合型, initial change fusion type. Describes phonetic change at the start of the word in counting expressions. Only available for a few hundred entries, mostly numbers. Values are N followed by a letter or number; most entries with this value are numeric.
- **fConType:** 語末変化結合型, final change fusion type. This is also used for counting expressions, and like iConType it is only available for a few hundred entries. Unlike iConType the values are very complicated, like `B1S6SjShS,B1S6S8SjShS`.
- **type:** Not entirely clear what this is, seems to have some overlap with POS.
- **kana:** 読みがな, this is the typical representation of a word in katakana, unlike pron. 講師 is コウシ.
- **kanaBase:** 仮名形基本形, this is the typical katakana representation of the lemma.
- **form:** 語形出現形, seems to be the same as `pron`.
- **formBase:** 語形基本形 seems to be the same as `pronBase`.
- **aType:** Accent type. This is a (potentially) comma-separated field which has the number of the mora taking the accent in 標準語 (standard language). When there are multiple values, more common accent patterns come first.
- **aConType:** This describes how the accent shifts when the word is used in a counter expression. It uses complicated notation.
- **aModType:** Presumably accent related but unclear use. Available for <25% of entries and only has 6 non-default values.
- **lid:** 語彙表ID. A long lemma ID. This seems to be a kind of GUID. There is usually one entry per line in the CSV, except that half-width and full-width variations can be combined.
- **lemma_id:** 語彙素ID. A shorter lemma id, starting from 1. This seems to be as unique as the `lemma` field, so many CSV lines can share this value.

# License

The modern Japanese UniDic is available under the GPL, LGPL, or BSD license,
[see here](https://ccd.ninjal.ac.jp/unidic/download#unidic_bccwj). UniDic is
developed by [NINJAL](https://www.ninjal.ac.jp/), the National Institute for
Japanese Language and Linguistics. UniDic is copyrighted by the UniDic
Consortium and is distributed here under the terms of the [BSD
License](./LICENSE.unidic).

The code in this repository is not written or maintained by NINJAL. The code is
available under the MIT or WTFPL License, as you prefer.
