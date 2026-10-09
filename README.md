# Online BME Landscape Explorer

An interactive view of online and hybrid master's programs in biomedical engineering and adjacent fields, built to support the design and positioning of a new online master's program at Boston University's Department of Biomedical Engineering.

## What's here

`landscape-scan.md` is the research document and the source of truth. Every figure in the app traces back to a sourced line in it, with a verification tag saying how solid that line is.

`data.json` is the machine-readable extract the app reads. It is generated from the scan and never carries a figure the scan doesn't contain.

`index.html` is the app. It has two views: a coverage heatmap for reading the landscape, and a program builder for testing a hypothetical BU program against the field.

`UPDATING.md` is the monthly refresh procedure, including the research prompt to use.

## Running it

Open `index.html` in a browser, or serve the folder and open it. It needs no build step and no network access.

## The two views

The heatmap puts programs in rows and BME subfields in columns, shaded by how deeply each program covers each subfield. Empty columns are curricular white space; crowded ones are contested ground. Filters narrow by category, format, state, and cost.

The builder lets you set credits, price, format, and a topic mix for a hypothetical BU program, then shows the total cost, the price band it lands in, the closest competitors, and which of your topics nobody else covers at depth.

## Reading the data honestly

Three distinctions carry most of the meaning.

Verification status separates what was read on an official page (`linked`) from what came from a research pass that cited official pages without the link being carried forward (`second-pass`) from what rests only on an aggregator site (`aggregator`). Treat anything but `linked` as needing confirmation before it leaves your office.

Null is not zero. A null cost means not published; a null topic depth means the scan doesn't say. A zero depth means the program genuinely doesn't cover that subfield. The heatmap renders these differently on purpose.

Costs carry the academic year they were published for. Tuition pages turn over in spring and summer, so a figure without its year is a figure you can't use.
