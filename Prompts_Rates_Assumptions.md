# Prompts, rates and confirmed assumptions

AI Productivity Challenge · Archicad MCP server `archicad-30-BinhNguyen`
Test model: Archicad sample Villa-Patio (2 storeys, 24 doors, 26 windows, 387 walls)

## Part 1 – doors: prompt used

1. `Check the doors in the open model: which ones are missing a fire rating, and which need renaming. Preview only, don't change anything yet.`

The AI answers with a preview only (doors to rename, doors needing a fire rating, what the user has to do). Nothing is written to the model until the user approves the preview.

## Part 2 – walls: prompts used

1. `Act as my QS. Price every wall in the open model with my US rates, wall by wall. First show me the quantities by layer and the assumptions you need me to confirm: which layer gets which rate, wall heights, hidden layers and what to leave out. Don't price yet.`
2. `Assumptions confirmed. Price it, put the cost table in Excel`

## Rates (USD, installed, low / mid / high)

The first matching row wins.

| Rate code | Applies to | Low | Mid | High | Unit |
|---|---|---|---|---|---|
| CLADDING | layer name contains "Cladding" or "fachada" | 269 | 431 | 592 | per m² |
| DRYWALL_1S | layer name contains "Ceiling" | 50 | 83 | 113 | per m² |
| HPL_PANEL | thickness up to 60 mm | 151 | 183 | 248 | per m² |
| DRYWALL_2S | 61–120 mm on a "Partition" layer | 62 | 108 | 145 | per m² |
| CMU_6 | 61–160 mm (all other) | 151 | 194 | 258 | per m² |
| CMU_8 | 161–230 mm | 194 | 237 | 344 | per m² |
| RC_WALL | over 230 mm | 394 | 652 | 1,001 | per m³ |
| RC_WALL formwork | both faces of an RC wall | 43 | 97 | 151 | per m² |

US market, 2026, national averages from public US cost guides; order-of-magnitude, not tender grade. The same rates are on the `Rates` sheet of the Excel file, where they can be edited.

## Assumptions confirmed before pricing

1. **Rate by layer, then thickness.** A cladding or ceiling layer decides the rate first; otherwise the wall thickness decides (table above).
2. **Hidden layers included:** `ENZ - Finish - Ceiling` (29 walls) and `ENZ - General - fachada 02` (3 walls).
3. **Left out (236 walls):** `ENZ - Site - Existing Buildings` (81), `ENZ - General - fachada 01 OLD` (136), `ENZ - Interior - Furniture` and `ENZ - Interior - Furniture Built In` (19). They stay in the Excel list with Include = N.
4. **Heights:** every wall is priced at its own modelled height, not one height per rate.
5. **Area basis:** net area of one face, openings deducted. RC walls are priced by net volume plus formwork on both faces.
6. **Renovation status ignored:** 386 walls are tagged "Existing" by the sample template; all are priced as new work.
7. **Currency:** USD only. Excludes paint, insulation, waterproofing, sales tax, general conditions, overhead and profit, and location factor.

## Result

- 387 walls listed one per row, 151 priced, about 910 m² net.
- **USD 164,471 low / 274,611 mid / 406,055 high.**
- Cost table: `01_Part2_Wall_Cost_Table.xlsx` (6 sheets, live formulas: change a rate or a Y/N flag and every total updates).
- The model was only read for Part 2; nothing was changed.
