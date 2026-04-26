# Floor Plan Room Mapping (Guesses)

Based on visual analysis of `planas.PNG` (630×402px architectural floor plan).
Annotated overlay saved as `plan-guess.PNG`.

## Room Zones

| Room (LT) | Room (EN) | Location in plan | Approx. coords (x1,y1,x2,y2) |
|---|---|---|---|
| Svetainė | Living room | Upper left — largest open area | 70, 25, 290, 255 |
| Virtuvė | Kitchen | Inside living area — central island feature | 130, 75, 215, 190 |
| Miegamasis | Master bedroom | Upper right, first bay | 290, 25, 390, 145 |
| Tėvų vonia | Parents' bathroom | Upper right, second bay | 390, 25, 460, 145 |
| Barboros / Sūnaus kambaris | Barbora's / Son's room | Upper right, third bay | 460, 25, 555, 145 |
| Darbo kambaris | Office / work room | Right-side vertical strip, upper | 555, 140, 625, 260 |
| Koridorius | Corridor / hallway | Horizontal strip dividing upper/lower | 70, 255, 555, 275 |
| Garažas | Garage | Lower left — 3 garage bay squares visible | 0, 270, 195, 378 |
| Tambūras | Entrance hall | Lower left, narrow strip beside garage | 195, 270, 255, 378 |
| Dukros kambaris | Daughter's room | Lower centre-left | 255, 270, 355, 378 |
| Alex kambaris | Alex's room | Lower centre | 355, 270, 460, 378 |
| Sūnaus kambaris | Son's room | Lower centre-right | 460, 270, 555, 378 |
| Vonia | Bathroom | Lower right strip | 555, 270, 625, 378 |

## Notes

- Coordinates are in pixels relative to `planas.PNG` (origin top-left).
- Room boundaries are guesses — correct them against the actual blueprint.
- The upper-right quadrant (x: 290–555, y: 145–255) is unlabelled — could be an open plan extension of Svetainė or a hallway.
- The left exterior strip (x: 0–70, y: 0–260) appears to be outside the building footprint.

## Related files

- `planas.PNG` — original floor plan
- `plan-guess.PNG` — annotated overlay with colour-coded zones
