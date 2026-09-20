NestClean
Langmuir Apollo / LaserControl
Version 1.2.2

Clean a nested knife-blank DXF for the laser, or array blanks onto a sheet
yourself. Download R12. Load that in LaserControl.

Nothing in this program uploads your DXF. Work stays on the PC that is
running it.


Open the Apollo copy
--------------------
Latest zip (bookmark this — always the current version):

  https://github.com/JPFava/nestclean-dist/releases/latest/download/NestClean.zip

Unzip. Right-click NestClean.html → Open with → Google Chrome.
If double-click opens Edge, use Open with Chrome.

Same pattern as Burl Plate. No Grok sign-in. Replace the old folder
when the version number changes.

On a phone, Open / Add DXF must go through Files or Downloads — not Camera
or Photos. Android used to open the camera because DXF is registered as an
image type.


Clean
-----
Open a nested sheet (or drop it on the window). NestClean strips the
wrapper LaserControl chokes on and writes R12: LINE, ARC, CIRCLE, layer 0,
color 7, Z = 0. Segments shorter than 0.001 in are dropped so LaserControl
does not report Missing Offsets on 1-segment slivers.

Export flavor (next to the buttons)
  LINE + ARC   Most primitive. Start here.
  POLYLINE     Closed contours, fewer entities.
  Lines only   No arcs. Last resort.

The file is ordered for the torch: holes, then cutouts, then the outer,
one blank at a time, row by row. If LaserControl still jumps, turn off
path optimize so it follows this order.

Close-gap welds open contours (Fusion sketch gaps). Raise it if paths
still show as open.

Move nest to origin puts the drawing in positive coordinates with a
small margin.


Why AutoCAD and Fusion keep failing
-----------------------------------
The nest is not the problem. LaserControl is allergic to the file wrapper.

AutoCAD 2013 / 2018 DXF carries CLASSES / OBJECTS baggage and leftover
POINT entities from ARRAY. The laser tries to pierce those dots.

Fusion LWPOLYLINE, empty sketch exports, and projected-from-face files
with negative coordinates are the usual follow-up mess.

Skip Fusion for the laser file. Array here (or in AutoCAD), download
R12, load that in LaserControl.


Nest
----
Open a single blank (or a nest — unique profiles show up under Blanks).
Set quantity. Set the sheet.

Sheet W / H     Stock size in inches.
Border          Keep-out from the plate edge.
Min gap         Floor, measured curve-to-curve (not box-to-box at the
                cone tips). After the pack, Spread makes that gap the
                same both ways and puts leftover plate on the borders.
                That extra air is what keeps the torch off a neighbor's
                cutout.

Allow 90° leftovers
                Long way first. Parts that do not fit long-way stand on
                end on the remaining strip.

Alternate 180°  Flip every other column (tip against handle along the row).
                Head-to-head and tail-to-tail each get their own pitch.
                Rows stay aligned so steel does not overlap. Leave it on.

Spread          Leftover plate goes into the gaps, not the borders. Min gap
                is the floor. Outer blanks sit on the keep-out. Leave it on.

Each blank shows "sheet holds N" for an empty plate. Type a quantity and
the nest updates as you type. If some do not fit, the box drops to how
many nested. The other blanks then show max N with the others from that
nest. Max fills that leftover.

Several DXFs
------------
Add DXF for each blank type. Quantities start at 0 so each shows its
own max. Set the first type, leftover updates on the others, set the
next, and so on. Nest packs in the order the blanks are listed.
90° leftovers only run on the last type so remnant plate goes
to the next DXF. Empty slots on an incomplete last row stay
usable — leftover is not only the rectangle outside the first nest.
Spread runs after that nest — it does not change how many fit.

Cut order in the DXF: holes → cutouts → outer, then the next blank,
row by row, standing leftovers last.


Chrome on this PC
-----------------
Chrome can package NestClean as its own window. Three-dot menu →
Cast, save, and share → Install NestClean. Or the install icon on the
right of the address bar. Pin it next to LaserControl.
