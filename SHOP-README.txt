NestClean
Langmuir Apollo / LaserControl
Version 1.2.14

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
one blank at a time. The head starts at the far end and steps across
the sheet. If LaserControl still jumps, turn off path optimize so it
follows this order. A partial sheet is Pick, Window, or Crossing, then
Write selected or Write all but selected.

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

Spread          Extra plate goes into the gaps. Mixed nests fill the
                leftover height (the empty top). 90° leftovers stay in a
                clear strip on the end so they do not sit on the 0° nest.

Each blank shows "sheet holds N" on an empty plate. After a nest, each
shows "N more" — how many of that DXF still fit in leftover plate.
Max adds that many without moving the blanks already nested.
If some do not fit, the box keeps the number you typed and the panel
lists "N Name not nested" per type. Change any quantity and leftover
for every type updates.

Several DXFs
------------
Add DXF for each blank type. Quantities start at 0 so each shows its
own max. Set the first type, leftover updates on the others, set the
next, and so on. Nest packs in the order the blanks are listed.
90° leftovers only run on the last type so remnant plate goes
to the next DXF. Empty slots on an incomplete last row stay
usable — leftover is not only the rectangle outside the first nest.
Spread runs after that nest — it does not change how many fit.

Cut order in the DXF and in a corrected .tap: holes, then cutouts, then
the outer. The next blank is across the sheet, starting at the far end
(high X) and stepping toward the near end. A finished column stays
behind the head.

Every outside is wound the same way. Every cutout is wound the opposite
way, so LaserControl's kerf offset stays on the scrap side. Circles stay
circles — a DXF circle has no direction.

Check a .tap
------------
After LaserControl saves the program, use Check LaserControl .tap.
White is the nest. Green is the Apollo path sitting on that same line.
Blue is the path where it does not meet the nest. Amber is a cut off
the nest. Red is nest with no cut. A correct sheet is green on the
profiles, with no amber and no red.
If the origins differ, NestClean slides the path onto the nest.
Offset X and Offset Y add more, in inches, if it is still short.
If Apollo turned the job, NestClean turns it back so it sits on the nest.
Lead-ins shorter than 0.35 in are ignored. Anything farther than
0.040 in from the line is a miss. Rapids (G0) are not cuts.

Write corrected .tap
--------------------
LaserControl does not follow the DXF order, so the cut order has to
be rewritten in the .tap. The new file:
  - moves a hole or cutout that has the right shape but the wrong place
  - drops a cut that is not on a blank
  - copies a missing hole from a hole Apollo did cut
  - cuts the insides of a blank, then its outline, then the next blank
The head starts at the far end and steps across the sheet. It does not
travel back over a blank that is already free to tilt. Pick, Window, or
Crossing on the sheet, then Write selected or Write all but selected,
to finish a partial sheet without letting LaserControl reorder the job.
If Apollo turned the job 90°, the corrected file stays turned that way
so it still runs. Feed, pierce, and laser on/off stay as Apollo wrote
them. Kerf offset on a cut that is already in the right place is left
alone. The original .tap is not changed.


Chrome on this PC
-----------------
Chrome can package NestClean as its own window. Three-dot menu →
Cast, save, and share → Install NestClean. Or the install icon on the
right of the address bar. Pin it next to LaserControl.
