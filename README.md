# Personal Archicad 30 MCP Tool v1.7.0

**AI Productivity Challenge · Parts 1 and 2 · Team VietCAD**

A local Python MCP server that connects Claude Code or Claude Desktop to the model open in Archicad through its built-in JSON API. Use it to review missing fire ratings and inconsistent element IDs, then turn modelled wall quantities into an early cost estimate.

## Install and connect

**Requirements** · Windows, Archicad 30 with a project open, Python 3.8+ on PATH, and Claude Code or Claude Desktop. Archicad 29 also worked in testing. The server uses the Python standard library and Archicad's local JSON API on ports 19723–19743.

1. Download and extract [the v1.7.0 package](05_archicad-30-BinhNguyen-mcp_v1.7.0.zip).
2. Run `install.cmd` from the extracted folder. It installs `server.py` under `%LOCALAPPDATA%\archicad-30-BinhNguyen\`, registers the server with the Claude clients it finds, backs up the Claude Desktop configuration before editing it, and runs a self-test.
3. Restart your Claude client. Ask it to run `archicad_status` and confirm that it reports your Archicad instance and its door, window, and wall counts.

Run `install.cmd` again to update or `uninstall.cmd` to remove the integration. If tools are missing in Claude Code, run `claude mcp list` to check registration. If several Archicad projects are open, pass the intended instance's `port`. In Teamwork, reserve elements before applying changes.

## Part 1 · Review fire ratings and element IDs

> Check the doors in the open model for missing fire ratings and IDs that need renaming. Show me a preview before applying any changes.

Use `find_doors_missing_fire_rating` to locate gaps by storey and `get_host_walls_of_doors` to inspect host-wall information. Apply project-specific rating rules, review the proposed changes, then approve the batch. The v1.7.0 opening tools also accept `elementType: "Window"`. The server also supports wall fire-resistance and Element ID updates.

Each write requires a valid `previewId` and `confirmedByUser: true`. The write tools update existing fire-rating or Element ID properties. Opening fire-rating batches fill empty values and preserve existing ratings. The project team determines which ratings apply.

**Villa-Patio result.** The model started with 24 doors missing ratings, 10 on the ground storey and 14 on the first. The demo resolved all 24 and standardized the duplicate and inconsistent door IDs. Nine corridor-related doors received EI 30 under the demo's project rules.

## Part 2 · Measure and price walls

> Act as my quantity surveyor. Show quantities by layer and ask me to confirm rates, heights, hidden layers, and exclusions before pricing.

`get_wall_quantities` summarizes the open model by layer or wall type. Review the quantities and agree on scope before running `price_walls`, which returns area-based low, mid, and high costs from the supplied rates. Both tools are read-only.

The submitted Excel estimate uses modelled heights and net face areas with openings deducted. It applies layer rules before thickness rules and prices reinforced-concrete walls by net volume plus formwork on both faces. The workbook contains the detailed wall list, editable rates and inclusion flags, and formula-driven summaries.

| Villa-Patio take-off | Result |
| --- | ---: |
| Walls listed | 387 |
| Walls priced | 151 |
| Walls excluded | 236 |
| Approximate net area priced | 910 m² |
| Low estimate | USD 164,471 |
| Mid estimate | USD 274,611 |
| High estimate | USD 406,055 |

The estimate uses early-stage US installed rates. See the [confirmed assumptions](Prompts_Rates_Assumptions.md) for hidden layers, exclusions, and the rate schedule.

## Submission files

| File | Contents |
| --- | --- |
| [MCP package](05_archicad-30-BinhNguyen-mcp_v1.7.0.zip) | Python server, Windows installer and uninstaller, and package documentation |
| [Wall cost workbook](01_Part2_Wall_Cost_Table.xlsx) | Assumptions, By layer, Rates, Elements Detailed, Rate Summary, and Wall Summary |
| [Prompts, rates, and assumptions](Prompts_Rates_Assumptions.md) | Reproduction prompts and confirmed estimating inputs |
| [Problem and time saved](MCP%20Description%20-%20Problem_and_Time_Saved.docx) | Challenge write-up, before-and-after results, and time comparison |
| [Challenge demo](Binh%20Nguyen%20-%20Archicad%20Challenge.mp4) | 3:49 walkthrough stored with Git LFS |

The challenge write-up compares about **12.5 hours of estimated manual work** with **22 minutes using the AI-assisted workflow** across both tasks.
