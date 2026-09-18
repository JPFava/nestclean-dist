NestClean
Langmuir Apollo / FireControl
Version 1.1.0

Clean a nested knife-blank DXF for the laser, or array blanks onto a sheet
yourself. Download R12. Load that in FireControl.

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


Clean
-----
Open a nested sheet (or drop it on the window). NestClean strips the
wrapper FireControl chokes on and writes R12: LINE, ARC, CIRCLE, layer 0,
color 7, Z = 0.

Export flavor (next to the buttons)
  LINE + ARC   Most primitive. Start here.
  POLYLINE     Closed contours, fewer entities.
  Lines only   No arcs. Last resort.

The file is ordered for the torch: holes, then cutouts, then the outer,
one blank at a time, row by row. If FireControl still jumps, turn off
path optimize so it follows this order.

Close-gap welds open contours (Fusion sketch gaps). Raise it if paths
still show as open.

Move nest to origin puts the drawing in positive coordinates with a
small margin.


Why AutoCAD and Fusion keep failing
-----------------------------------
The nest is not the problem. FireControl is allergic to the file wrapper.

AutoCAD 2013 / 2018 DXF carries CLASSES / OBJECTS baggage and leftover
POINT entities from ARRAY. The laser tries to pierce those dots.

Fusion LWPOLYLINE, empty sketch exports, and projected-from-face files
with negative coordinates are the usual follow-up mess.

Skip Fusion for the laser file. Array here (or in AutoCAD), download
R12, load that in FireControl.


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

Alternate 180°  Flip every other blank in a row.

Spread          Equal gap both ways after the count. Leave it on unless
                you want a tight pack.

Cut order in the DXF: holes → cutouts → outer, then the next blank,
row by row, standing leftovers last.


Chrome on this PC
-----------------
Chrome can package NestClean as its own window. Three-dot menu →
Cast, save, and share → Install NestClean. Or the install icon on the
right of the address bar. Pin it next to FireControl.
