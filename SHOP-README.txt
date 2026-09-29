NestClean
Langmuir Apollo / LaserControl
Version 2.0.2

Corner feed (this version)
--------------------------
v1.2.18 is the last release without it. Slow corners is off until you
check it. Leave it unchecked for the same TAP as that older release.

Slow corners runs after the cut order is rewritten. It only changes
feed near a direction change. It reads the F already in the file
(often the header) and scales it, then puts that feed back.
It does not change power, pierce, height, or beam on/off.
Small holes stay at the programmed feed.
Material picks a starting scale. The four numbers under it are saved
on this browser. Thicker plate or oxygen should sit closer to 1.0.

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
one blank at a time. Numbers on the nest are that order. 1 is the
rightmost column (maximum Y). The whole column is cut before the
gantry beam steps to a lower Y. If LaserControl still jumps, turn off
path optimize so it follows this order. After the nest is built, Pick, Window, or Crossing
selects blanks with no LaserControl file. DXF of selected, or DXF of
the rest, is only those blanks. A tap of the same set is there only
after you have loaded a .tap.

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

Sheet W / H     Stock size in inches. These boxes stay the size you typed.

Rotate plate 90°
                Starts at 90°. A long blank then lies along the head
                (red +X, down the sheet) and the gantry beam stays at
                one Y while that blank is cut. The blue dot stays at
                the upper left. Click to turn the steel the other way.
                The laser DXF is written in those machine axes. The
                sheet size boxes stay the stock you typed.
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

Mirror rows      Turn every other row over so the tall tab sits in the
                notch beside the other row's tab. Spread does not open
                those rows. If Alternate 180° would loosen that nest,
                the tighter tab nest is used when it fits the same count.

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
the outer. The head rides on the Y gantry, so every blank in a column
is finished before Y steps down. Columns run from maximum Y to minimum Y.

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
The head finishes one Y column, starting at the right, before the beam
moves left. It does not travel back over a blank that is already free
to tilt. Pick, Window, or
Crossing on the sheet, then Write selected or Write all but selected,
to finish a partial sheet without letting LaserControl reorder the job.
If Apollo turned the job 90°, the corrected file stays turned that way
so it still runs. Pierce and laser on/off stay as Apollo wrote them.
With Slow corners on, feed is scaled down at corners and restored
after. Kerf offset on a cut that is already in the right place is left
alone. The original .tap is not changed.

Steps
-----
Load, Clean, Nest, Profile, and Export. Only the open step is on screen
so the sheet stays large. Profile holds kerf, feed, slower holes, and
slow corners. Export writes the DXF and the TAP.

Cut order
---------
1 is the far-right blank. The head finishes every blank in that column
before the beam steps left. It does not sweep across a row. Side-by-side
arms stay in order, right to left.

Write Apollo .tap
-----------------
Skip LaserControl CAM. With a DXF or a nest on screen, Write Apollo .tap
makes a program the Apollo will run.

  - Inches, absolute, one feed in the header.
  - Laser on is M64 P0. Laser off is M65 P0. The same header handshake
    LaserControl writes (M64 P1 / M65 P1) is kept.
  - No power, no pierce time, no focus, no cut height. Select the
    material profile on the Apollo (for 3 mm 14C28N that is
    0125 14C28N N2 035 2.0 -1.4) and set that profile's kerf to 0.
    The Kerf width box is the full slot the beam removes (measured hole
    ID minus the path diameter). Path 0.500 and ID 0.510 means enter
    0.010. NestClean moves each edge by half of that: outline outward,
    hole inward. Zero leaves the beam on the CAD line.
  - A hole is pierced in the slug, then a tangent arc joins the circle.
    The outline is pierced in the scrap at the middle of the longest
    straight side, never on a corner or a small fillet.
  - Insides are cut before the outline. Blanks follow the gantry:
    one Y column, from the right, before the beam steps left.
  - Slower holes and Slow corners stay off until you check them.
    Slower holes use 0.67 times the cut feed on the hole only, then
    put the cut feed back. Slow corners only changes feed.

Check LaserControl .tap and Write corrected .tap still work when you
already have a program from LaserControl. Write Apollo .tap does not
need that file.



Chrome on this PC
-----------------
Chrome can package NestClean as its own window. Three-dot menu →
Cast, save, and share → Install NestClean. Or the install icon on the
right of the address bar. Pin it next to LaserControl.
