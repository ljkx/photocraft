# Layered TIFF fixtures

Four tiny TIFFs with Photoshop layer data (tag 37724) written by an independent implementation,
psdtags 2026.1.29 with tifffile 2026.9.20, so `tests/tiff_layers.rs` checks PhotoCraft's reader
against a second writer rather than only against itself.

| File | Byte order | Depth |
|---|---|---|
| `layered-le-8bit.tif` | Intel (`II`, blocks `MIB8`) | 8 |
| `layered-be-8bit.tif` | Motorola (`MM`, blocks `8BIM`) | 8 |
| `layered-le-16bit.tif` | Intel | 16 |
| `layered-be-16bit.tif` | Motorola | 16 |

Contents (identical across the four): a 6 × 4 RGB canvas at 300 dpi whose composite is the
Background ramp, with an unassociated alpha sample, and two layers:

- **Background** at (0, 0), 6 × 4, Normal, opacity 255, RLE channels. Pixel (x, y) is
  `R = (40x + 1) mod 256`, `G = (60y + 3) mod 256`, `B = (17xy + 1) mod 256`, alpha 255.
- **Upper é** at (1, 1), 4 × 3, Multiply, opacity 200, ZIP channels. Pixel (x, y) is
  `R = (40x + 7) mod 256`, `G = (60y + 21) mod 256`, `B = (17xy + 7) mod 256`, alpha
  `(80x) mod 256`.

16-bit files hold the same values scaled by 257. Each layer carries a `luni` Unicode name; the
layer list has `has_transparency` off, so the composite's alpha sample is an alpha channel by the
PSD rules; the user mask is RGB red at 50 %.
