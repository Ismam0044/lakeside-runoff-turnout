# Lakeside 2026 City Council Runoff turnout

This project looks at how the Lakeside City Council runoff is going so far. The runoff started on June 1 and the data here runs through June 4. Lakeside sits across two counties, Northgate and Southfield, and each county sends its voter files in its own format, so a big part of the work was figuring out how the files connect before measuring anything.

The notebook does the whole thing start to finish. It loads the raw files, shows what is messy in them, cleans and combines everything, works out turnout by precinct, and then builds the report image and the map.

- Notebook: [`lakeside_runoff_analysis.ipynb`](https://github.com/Ismam0044/lakeside-runoff-turnout/blob/main/lakeside%20runoff%20turnout.ipynb)
- Report image: [`outputs/lakeside_runoff_report.png`](https://github.com/Ismam0044/lakeside-runoff-turnout/blob/main/lakeside_runoff_report.png)
- Live map: [https://ismam0044.github.io/lakeside-runoff-turnout/](https://ismam0044.github.io/lakeside-runoff-turnout/)
## How to run this

First, make a new folder on your computer for this project. Then download all the data files from this GitHub repo into that folder. You need all of these:

registered_voters.csv
northgate_early_inperson.csv
southfield_early_inperson.xlsx
mail_ballots.xlsx
first_round_may.csv
precincts.geojson


Next, open `lakeside_runoff_analysis.ipynb` and upload it to Jupyter Notebook. Now set the file paths so the notebook can find your files. Go through the cells in this order:

- First cell: in the `DATA` variable, put the path to the folder you created. This lets the notebook find all the other files in that folder.
- Second cell: put the path to the `registered_voters.csv` file so the data shows up.
- Fourth cell: in the `northgate_raw` variable, put the path to `northgate_early_inperson.csv`.
- Sixth cell: put the path to the `southfield_early_inperson.xlsx` file.
- Seventh cell: in the `southfield_raw` variable, put the path to `southfield_early_inperson.xlsx`.
- Eighth cell: in the `mail_raw` variable, put the path to `mail_ballots.xlsx`.
- Ninth cell: in the `first_round_raw` variable, put the path to `first_round_may.csv`.
- Tenth cell: in the `geo_raw` variable, put the path to `precincts.geojson`.
- Last cell (right before the end): in the `precinct_map` variable, put the path to `precincts.geojson` again.

Once the paths are set, run the cells from top to bottom. When it finishes it writes the report to `outputs/lakeside_runoff_report.png` and the map to `docs/index.html`.

Tip: if you keep all the data files in one folder and set the `DATA` variable to that folder, the rest of the cells will already point to the right files, so you mostly just have to set that first path.

## What the numbers say

By June 4, about 276 of 3,615 Lakeside voters had voted in the runoff. That is roughly 7.6 percent, or about a quarter of how many showed up in the first round. The thing that stood out to me is that most of these runoff voters, 181 of the 276, did not vote in May at all. So the runoff is not just the May crowd coming back, it is bringing in different people. There are also 1,074 people who voted in May but have not returned yet, which is a big group still left to vote. Most ballots so far are in person rather than mail. Southfield 30 has the highest turnout and Southfield 210 the lowest, and Stonebriar is ahead of the other neighborhoods on every measure.

## Things in the data I had to deal with

Some were easy to spot, some I only caught after looking closely:

- The roll had exact duplicate rows, so I dropped them.
- The city name was typed several different ways, so I uppercased and stripped it.
- The roll also lists people from other towns. Since this is a Lakeside race and none of them appear in any vote file, I kept Lakeside voters only.
- Party had five different spellings of "no party," so I put them all under one Unaffiliated label.
- Sex had blanks and a `U`, which I treated as Unknown.
- Age had impossible values like 0 and 150, so I treated anything outside 18 to 110 as missing.
- Precinct 30 exists in both counties, so a plain number is not unique. I keyed each precinct by county plus number.
- Two precincts, Northgate 253 and Southfield 264, do not have their own shape in the map file. The reference report pairs them with a neighbor ("220 / 253", "30 / 264"), so I folded them into 220 and 30.
- Subdivision names carried phase, section, and unit tags plus codes like (CFR), so I trimmed them to a base name.
- Northgate's dates were a mix of `2026-06-02` and `06/02/2026` in the same column, so I parsed them with `format="mixed"`.
- The Southfield Excel file has a "Total records" title row at the top, so I skipped it with `skiprows=1`.
- The mail file had 5 repeated rows, and 13 people showed up in both an in-person file and the mail file. I counted each voter once and kept the in-person record when someone was in both.
- None of the vote files include a precinct, so I joined the precinct and the other details back from the roll using the voter id.

One note on framing: the example report was partisan ("Frisco Democrat"), but this is a non-partisan race, so I report total turnout across all registered Lakeside voters instead of splitting it by party.

## The map

The map file is `index.html`. To put it online I turned on GitHub Pages from Settings, then Pages, and set the source to the `main` branch and the root folder. After that it shows up at https://ismam0044.github.io/lakeside-runoff-turnout/
