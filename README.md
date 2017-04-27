# soas-taggers

This repo contains taggers and taggings that arose as part of an AHRC-funded
project based at SOAS from 2012-2015. For further details, please see
[Tibetan in Digital Communication](https://www.soas.ac.uk/cia/tibetanstudies/tibetan-in-digital-communications/).

## Part-of-speech taggers

The *taggers* directory includes two part-of-speech taggers.
Both taggers take a lexical tagging (see below) as input, and 
then remove those tags which the tagger knows to be incorrect.
Some words are left with multiple candidate tags.
Because these taggers implement the same rules, in theory
they should give the same results. However, their outputs have
not been systematically compared, and so there are likely
to be differences between them.

### The regex tagger

The *regex tagger* consists of a series of rules, one per line.
Each rule has a left side and a right side, separated by
the greater than symbol. The left side is the pattern that must
be matched in the input, and the right side is the replacement.

```
(གིས་\|\S*)\[case\.gen\](\S*)(?!\s+\S+\[n\.\S+) > $1$2
```

The SOAS project used Java to apply the rules in sequence to
a *horizontally formatted lexical tagging*; therefore, the rules are stated using
[Java's conventions](https://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html).
The regex tagger is now deprecated. Its rules are difficult to read 
and maintain, and we advise use of the *vislcg3 tagger* instead.

### The vislcg3 tagger

In order to use the vislcg3 tagger, its binaries must first be compiled 
for your system. To do so, follow the instructions linked from the 
[VISL CG-3 Development Information](http://visl.sdu.dk/cg3.html) page.

After installing vislcg3, the tagger needs to be compiled:

```
cg-comp cg3.txt pos.cg
```

The compiled constraint grammar can then be invoked as follows to tag 
*vertically formatted lexical taggings* (see below):

```
vislcg3 -g pos.cg -I lex_vislcg_74.txt cg3-74.txt
```

## Lexical taggings

As noted above, the two part-of-speech taggers can only be applied to
lexical taggings. A lexical tagging is a corpus that has been divided into
words with candidate parts of speech. The pos-tagger then removes those
tags which it knows to be incorrect.

For testing and development, the *taggings* directory of this repo includes
lexical taggings for the following texts:

* མཛངས་བླུན་ཞེས་བྱ་བའི་མདོ། (74)
* མར་པ་ལོ་ཙཱའི་རྣམ་ཐར། (444)
* བུ་སྟོན་ཆོས་འབྱུང་། (11842)
* མི་ལའི་རྣམ་ཐར། (11854)

The number in parentheses is the identifier for the text, which is also contained
in the filenames. For example, the file *lex_74.txt* is the horizontal lexical
tagging for མཛངས་བླུན་ཞེས་བྱ་བའི་མདོ།, and the file *lex_vislcg_74.txt* is the vertical
lexical tagging of the same text.
