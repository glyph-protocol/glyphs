`version 0.0.1`

# Glyphs

Glyphs allow Bitcoin transactions to etch, mint, and transfer Bitcoin-native digital commodities.  They are very similar to the Runes protocol, but rely soley on OP_RETURNs, and do not support inscriptions.  Glyphs are designed to be compatible with nostr npubs and bitcoin taproot.

Whereas every glyph is unique, every unit of a glyph is the same. They are interchangeable tokens, fit for a variety of purposes.

## Glyphstones

Glyph protocol messages, called glyphstones, are stored in Bitcoin transaction outputs.

A glyphstone output's script pubkey begins with an OP_RETURN, followed by OP_13, followed by zero or more data pushes. These data pushes are concatenated and decoded into a sequence of 128-bit integers, and finally parsed into a glyphstone.

A transaction may have at most one glyphstone.

A glyphstone may etch a new glyph, mint an existing glyph, and transfer glyphs from a transaction's inputs to its outputs.

A transaction output may hold balances of any number of glyphs.

Glyphs are identified by IDs, which consist of the block in which a glyph was etched and the index of the etching transaction within that block, represented in text as BLOCK:TX. For example, the ID of the glyph etched in the 20th transaction of the 500th block is 500:20.

## Etching

Glyphs come into existence by being etched. Etching creates a glyph and sets its properties. Once set, these properties are immutable, even to its etcher.

### Name

Names consist of the letters A through Z and are between one and twenty-six letters long. For example UNCOMMONGOODS is a glyph name.

Names may contain spacers, represented as bullets, to aid readability. UNCOMMONGOODS might be etched as UNCOMMON•GOODS.

The uniqueness of a name does not depend on spacers. Thus, a glyph may not be etched with the same sequence of letters as an existing glyph, even if it has different spacers.

Spacers can only be placed between two letters. Finally, spacers do not count towards the letter count.

### Divisibility

A glyph's divisibility is how finely it may be divided into its atomic units. Divisibility is expressed as the number of digits permissible after the decimal point in an amount of glyphs. A glyph with divisibility 0 may not be divided. A unit of a glyph with divisibility 1 may be divided into ten sub-units, a glyph with divisibility 2 may be divided into a hundred, and so on.

### Symbol

A glyph's currency symbol is a single Unicode code point, for example $, ⧉, or 🧿, displayed after quantities of that glyph.

101 atomic units of a glyph with divisibility 2 and symbol 🧿 would be rendered as 1.01 🧿.

If a glyph does not have a symbol, the generic currency sign ¤, also called a scarab, should be used.

### Premine

The etcher of a glyph may optionally allocate to themselves units of the glyph being etched. This allocation is called a premine.

### Terms

A glyph may have an open mint, allowing anyone to create and allocate units of that glyph for themselves. An open mint is subject to terms, which are set upon etching.

A mint is open while all terms of the mint are satisfied, and closed when any of them are not. For example, a mint may be limited to a starting height, an ending height, and a cap, and will be open between the starting height and ending height, or until the cap is reached, whichever comes first.

### Cap

The number of times a glyph may be minted is its cap. A mint is closed once the cap is reached.

### Amount

Each mint transaction creates a fixed amount of new units of a glyph.

### Start Height

A mint is open starting in the block with the given start height.

### End Height

A glyph may not be minted in or after the block with the given end height.

### Start Offset

A mint is open starting in the block whose height is equal to the start offset plus the height of the block in which the glyph was etched.

### End Offset

A glyph may not be minted in or after the block whose height is equal to the end offset plus the height of the block in which the glyph was etched.

## Minting

While a glyph's mint is open, anyone may create a mint transaction that creates a fixed amount of new units of that glyph, subject to the terms of the mint.

## Transferring

When transaction inputs contain glyphs, or new glyphs are created by a premine or mint, those glyphs are transferred to that transaction's outputs. A transaction's glyphstone may change how input glyphs transfer to outputs.

## Edicts

A glyphstone may contain any number of edicts. Edicts consist of a glyph ID, an amount, and an output number. Edicts are processed in order, allocating unallocated glyphs to outputs.

## Pointer

After all edicts are processed, remaining unallocated glyphs are transferred to the transaction's first non-OP_RETURN output. A glyphstone may optionally contain a pointer that specifies an alternative default output.

## Burning

Glyphs may be burned by transferring them to an OP_RETURN output with an edict or pointer.

## Cenotaphs

Glyphstones may be malformed for a number of reasons, including non-pushdata opcodes in the glyphstone OP_RETURN, invalid varints, or unrecognized glyphstone fields.

Malformed glyphstones are termed cenotaphs.

Glyphs input to a transaction with a cenotaph are burned. Glyphs etched in a transaction with a cenotaph are set as unmintable. Mints in a transaction with a cenotaph count towards the mint cap, but the minted glyphs are burned.

Cenotaphs are an upgrade mechanism, allowing glyphstones to be given new semantics that change how glyphs are created and transferred, while not misleading unupgraded clients as to the location of those glyphs, as unupgraded clients will see those glyphs as having been burned.

## Magic Numbers

Glyphs was originally going to use its own magic number.  But instead it will reuse runes OP_13, for backwards compatibility.  Previous to Aug 9 on testnet OP_16 was used for testing.

Optionally in a Glyph you can use enum 123=1 (from the monas heiroglyphica) to indicate a glyph.

## Status

Given that glyphs are now backwards compatible with Runes they can be considered live.  Glyphs reserves the right to deviate from runes if anything is considered harmful to bitcoin, such as large inscriptions taking up block space.

## See Also

- [Conventions](./CONVENTIONS.md)
