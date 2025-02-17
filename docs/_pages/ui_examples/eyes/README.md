---
layout: default
title: Can't Take My Eyes Off of You
grand_parent: User Edition
parent: UI Examples
nav_order: 2
---
# Can't Take My Eyes Off of You
{: .no_toc }

This case is one of the examples discussed on [issue #33](https://github.com/Audiveris/audiveris/issues/33)
posted on Audiveris forum, dedicated to the support of drum notation.

[PDF available here](https://github.com/Audiveris/audiveris/files/8421680/Can.t_Take_My_Eyes_Off_of_You-Drums%2BClap.pdf)
{: .btn .text-center }

Main score specificities:
- Book of 3 sheets
- Drum notation on 5-lines staves and 1-line staves
- Multiple-measure rests
- Measure repeats

Main UI actions:
- Selection of proper book parameters for drum notation
- Customization of drum-set mapping
- Fix missing rests in original input
- A few symbols to fix
- Edition of logical parts

---
Table of contents
{: .text-delta }

1. TOC
{:toc}
---

## Input

-- Note: We can ask our browser to display each of these sheet images in a separate tab --

| Sheet#1 | Sheet#2 | Sheet#3 |
| :---: | :---: | :---: |
| ![](./sheet1.png) | ![](./sheet2.png) | ![](./sheet3.png) |

## Book parameters

Based on a first look at input score, we can select proper book parameters,
via the ``Book | Set Book Parameters`` dialog:

![](./book_parameters.png)

We select the whole book tab, not just the first sheet tab, because selections will
need to apply to all sheets in book.

Here are the modifications made:
- We select a ``Serif`` font for better consistency with input font.
- For OCR, ``eng`` language is all we need
- We select **both** 1-line percussion and 5-line percussion
- We don't keep lyrics, since there are none in this score

## Customized drum-set

Since we are dealing with drum notation, we check the default drum-set.xml specifications.
It is available in Audiveris ``res`` (resources) folder.
Its content is also displayed in [this handbook section](../../specific/drums.md#appendix).

The default specifications should be OK for the 5-line staff (named Drumset in the input).

However, the 1-line staff in the input is named "Hand Clap", so we will specify such
hand clap "instrument" for oval on mid-line (pitch-position 0).

In default ``drum-set.xml``, we have (excerpt):
```xml
    ...
  <staff line-count="1">
    <entry pitch-position="0"  motif="oval"    sound="null"/> <!-- Just a place-holder -->
  </staff>
    ...
```
Which basically means that no instrument is defined for a 1-line percussion staff.

So, we create a "personal" ``drum-set.xml`` that provides one overriding definition:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<!-- My personal drum-set.xml-->
<drum-set>
  <staff line-count="1">
    <!-- Specific definition for Hand_Clap on the 1-line staves -->
    <entry pitch-position="0" motif="oval"  sound="Hand_Clap"/>    
  </staff>
</drum-set>
```
And we put this file in user Audiveris ``config`` folder.  
On my PC, path to this folder is precisely:
> C:\Users\herve\AppData\Roaming\AudiverisLtd\audiveris\config

And that's it.

## Raw results

We launch the whole book transcription, for example via (``Book | Transcribe Book``).

Two minutes later, we get:

| Sheet#1 | Sheet#2 | Sheet#3 |
| :---: | :---: | :---: |
| ![](./sheet1_raw.png) | ![](./sheet2_raw.png) | ![](./sheet3_raw.png) |

## Manual corrections

### Sheet #1

We have 3 measures displayed with a pink background, which means an error detected in RHYTHM step.
Their (local) ids are measures 4 and 8 in first system and measure 27 in last system.

We use ``View | Show score Voices`` so that each voice is displayed in a specific color.

|| Measures 3 & 4 |
| :---: | :---: |
| Before | ![](./sheet1_m3_4.png)|
| After  | ![](./sheet1_m3_4_ok.png)|

Measure 3: We observe a missing whole rest at the beginning of measure.  
- Detection failed because the whole rest is located out of staff height.  
- We select the glyph and assign it the WHOLE_REST shape using the
``Rests`` palette in shape board.

Measure 4: we can see the input lacks 2 quarter rests. 
This is especially obvious when compared with measure 3.
- So, we use a drag n' drop to manually insert these missing quarter rests.

Measures 7 and 8: They are identical to measures 3 and 4, they need identical corrections.

|| Measure 27|
| :---: | :---: |
| Before | ![](./sheet1_m27.png)|
| After  | ![](./sheet1_m27_ok.png)|

Measure 27: A cross-head is missing, replaced by a strong accent.  
- We delete the strong accent
- We select the stem glyph and assign it the stem shape
- We then insert a cross-head dragged from the ``HeadsAndDot`` palette in shape board.  
- We also assign the accent just above this note.

|| Measure 31|
| :---: | :---: |
| Before | ![](./sheet1_m31.png)|
| After  | ![](./sheet1_m31_ok.png)|

Measure 31: This last measure contains a direction word ("sloppily") OCR'd as "s oppi y".  
- To fix this, the best way is to grab this item with a lasso, which selects the item
as well as the underlying glyphs.  
- We then delete the item
- With the underlying glyphs still selected, we manually run OCR
via a double click on the "``text``" button in the ``Physicals`` set of shape board.

### Sheet #2

Just one measure (29) displayed in pink.

|| Measure 29|
| :---: | :---: |
| Before | ![](./sheet2_m29.png)|
| After  | ![](./sheet2_m29_ok.png)|

This is due to an 8th rest mistaken with a pair of augmentation dots.  
- With a lasso, we select the 8th rest underlying glyphs as well as the two dot items,
- We deassign the selected items,
- And click on the EIGHTH_REST button which appears at the top of glyph classifier board.

|| Measure 4|
| :---: | :---: |
| Before | ![](./sheet2_m4.png)|
| After  | ![](./sheet2_m4_ok.png)|

Measure 4: A missing crescendo wedge.  
- We select the underlying glyph and use a double-click on the crescendo button
in the ``Dynamics`` palette of shape board.

Similar action for another missing crescendo below measure 12.

### Sheet #3

|| Measure 5|
| :---: | :---: |
| Before | ![](./sheet3_m5.png)|
| After  | ![](./sheet3_m5_ok.png)|

Measure 5: A triple forte (fortississimo) not recognized, because Audiveris
doesn't go beyond fortissimo. [^fortississimo] 
- So, we manually select just a fortissimo, for lack of better choice.

Measure 6: a "sloppily" direction OCR'd as "s oppy y".
- See similar action in sheet 1.

Measure 10: a missing crescendo sign.
- See similar action in sheet 2.

|| Measure 16|
| :---: | :---: |
| Before | ![](./sheet3_m16.png)|
| After  | ![](./sheet3_m16_ok.png)|

Measure 16: An "8" character mistaken with the start of an octave shift.  
- We simply delete it.

|| Measure 25|
| :---: | :---: |
| Before | ![](./sheet3_m25.png)|
| After  | ![](./sheet3_m25_ok.png)|

Measure 25: A missing quarter.  
- We select the stem glyph and assign it via a double-click on the stem button
in the "``Physicals``" palette in shape board.  
- Then we drag a cross-head from the "``HeadsAndDot``" palette in shape board into proper location.

## Logical parts

If we naïvely export our work to say MuseScore, we get something that starts like:

![](./musescore_4_parts.png)

That is, for OMR engine we have 4 separate logical parts:
- D. Set
- Hd. Clp.
- Drumset
- Hand Clap

And indeed, if within Audiveris we open the
[logical parts editor](../../specific/logical_parts.md#edition-of-logical-parts),
we get this dialog:

![](./logicals_edition.png)

For the OMR engine limited capabilities, there is no way to detect that "D. Set" and "Drumset"
refer to the same logical part.

Let's do this manually:
1. We select the first logical (D. Set) and copy its name
2. We then select the third logical (Drumset) and paste into the Abbreviation field the copied name
3. We do the same between (Hd. Clp.) logical and "Hand Clap" logical
4. We can now remove the first 2 logicals (D. Set and Hd. Clp.)
5. We save (and thus lock) the logicals configuration

![](./logicals_edited.png)

We can finally apply the new configuration, with the logicals now locked:

![](./parts_collation.png)

No mapping warning is issued.

Export to MuseScore or Finale is now OK.  
First page in Finale looks like:

![](./finale_export.png)

[^fortississimo]: We will need to add fortississimo shape to Audiveris and retrain its glyph neural network with enough representative samples.