# Updating the BME Online Landscape App

Two files drive everything. `landscape-scan.md` is the written research document and the source of truth for prose, sourcing, and verification status. `data.json` is the machine-readable extract that the app reads. `index.html` reads `data.json` and nothing else.

Rule: never edit `data.json` without making the same change in `landscape-scan.md`, and never let a figure into `data.json` that isn't in the scan. The point of the pair is that every number in the app traces back to a sourced line in the document.

## Monthly refresh, short version

Open a new chat in the Online BME Masters project, attach `landscape-scan.md`, and ask for a delta scan rather than a fresh one. The delta prompt is below. Then apply the changes to both files and push.

## Monthly refresh, step by step

**1. Delta research.** New chat in the project, attach `landscape-scan.md`, use the delta prompt at the bottom of this file. Opus at medium effort is right for this one, because it is live research rather than code generation. Expect a changed-items list, not a rewritten document.

**2. Update the scan.** Apply the deltas to `landscape-scan.md` by hand or by asking Claude to produce edited sections. Keep the verification tags honest: anything the refresh found but didn't link gets marked "second pass" or "unverified," not "linked."

**3. Regenerate or patch `data.json`.** For a handful of changes, edit the JSON directly; the fields are plain. For a large batch, attach both the updated scan and the current `data.json` and ask Claude to output a new `data.json` that preserves every existing `topicDepth` rating unless the scan's text changed for that program. The depth ratings are the part you've hand-corrected, so protect them.

**4. Check and push.** Open `index.html` locally, confirm the program count and that no card shows "undefined," then commit with a dated message and push.

## What usually changes month to month

Tuition pages update on an academic-year cycle, so most cost changes land in spring and summer. Announced programs are the live ones to watch: Michigan's AI+ BME MEng (Fall 2027 start, Winter 2027 certificate, online timing unconfirmed), Yale's medical AI master's (first applications early 2027), and Florida's AI in biomedical and health sciences. New entrants tend to surface as university news releases rather than catalog changes, so the delta prompt searches announcements specifically.

## Fields in data.json

Each program object: `id`, `name`, `university`, `degree`, `category` (direct, adjacent, residential-signal, pathway-signal, absent), `format`, `credits`, `courses`, `totalCost`, `costHigh`, `costNote`, `perCredit`, `costYear`, `deliveryModel`, `launch`, `status` (linked, second-pass, aggregator), `sourceUrl`, `city`, `state`, `admitting`, `notes`, and `topicDepth`, an object keyed by the thirteen subfields with values 0 to 3 or null.

`courses` is a count of courses and is never converted to credits. `costHigh` is a second, higher price tier where the scan gives one, and `costNote` says what the two prices are (for Purdue, "resident table vs higher tier").

`admitting` defaults to true; set it false for programs the scan says are not currently enrolling, and exclude those from cost views and competitor scoring. Category `absent` marks universities checked and found to have no online BME program; those rows stay out of the heatmap and all scoring.

Null means not published or not determined. Zero means the program genuinely doesn't cover it. Keep those distinct; the heatmap renders them differently and the distinction is the whole point of the white-space read.

## Delta prompt for next month

Paste this into a new project chat with `landscape-scan.md` attached.

```
Attached is my current landscape scan of online and hybrid master's programs in biomedical engineering and adjacent fields, last updated [DATE]. Do a delta check, not a new scan. I want what changed, not a rewritten document.

Check, in this order:
1. Announced programs in the scan: Michigan's MEng in AI Engineering BME concentration (start date, whether the online option is confirmed), Yale's MHS in Medical Artificial Intelligence, and Florida's MS in AI in Biomedical and Health Sciences. Have dates, formats, or costs firmed up?
2. Tuition and credit changes at every direct competitor listed. Check the official bursar or program page for the current academic year.
3. New entrants: search university news releases and catalog changes from the last [N] weeks for newly announced or newly launched online or hybrid master's in biomedical engineering, bioengineering, medical device engineering or development, medtech innovation, digital health engineering, AI in medicine from an engineering school, and regulatory science with a device track.
4. Programs that stopped admitting, paused, or closed.
5. Any item the scan flags as unverified or "second pass" that you can now confirm with an official page link, especially Colorado State, Florida Atlantic, Louisville, and Minnesota's Medical Device Innovation MS.

Output a changed-items list only. For each: program, field, old value, new value, source link, and whether this changes the scan's conclusions about price bands or white space. If nothing changed for a program, don't list it. Never invent a citation; if you can't verify a figure, say so rather than estimating.
```
