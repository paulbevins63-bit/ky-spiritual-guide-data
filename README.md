# County guide data

One file per state. The app downloads the one it needs and caches it:

    https://raw.githubusercontent.com/paulbevins63-bit/ky-spiritual-guide-data/main/guide-data/<st>.json

Kentucky also ships inside the APK, so somebody who installs the app with no
signal — a realistic way to arrive at an app like this — still gets help.

## Why these files exist

The guides used to be parsed on the user's phone: tap, download a PDF, run a
parser, write whatever came out. That put every parser defect on every handset,
made a regex fix a Play release, and meant nobody could see a county's rows
without a phone fetching that county. Four defects shipped that way. One handed
a domestic violence shelter the street address of the charity listed above it in
the source document.

Parsing moved to a desktop, over the whole corpus at once, where the output can
be read before anyone depends on it. Fixing a bad row is now regenerating a
file, not shipping an app update.

## How a file is made

    python extract_corpus.py --state XX        # downloaded PDFs -> text
    ./gradlew :personal-assistance:testDebugUnitTest \
        --tests "*GuideCorpusBuilder*" \
        -Dguide.corpus.in=out/corpus -Dguide.corpus.out=out/parsed
    python build_state_data.py --state XX      # per-county -> one state file

The middle step runs the SHIPPING parser — the same `GuideDocumentParser` and
the same `toAppResource` the app uses — so there is no second implementation to
drift out of agreement with the first.

## Schema

    { "schema_version": 1, "state": "KY", "generated": "2026-07-31",
      "counties": [ { "fips": "21093", "county": "Hardin",
                      "resources": [ { "label", "category", "value", "isUrl",
                                       "streetAddress", "zipCode", "city",
                                       "note", "source" } ] } ] }

`schema_version` gates the whole file. A file written by a newer generator is
rejected outright rather than read optimistically, because a partly-understood
resource list is worse than none — the reader cannot tell which half they have.

## The one rule that is not negotiable

A domestic violence resource never carries a street address or a zip. It is
enforced in three places on purpose: in `toAppResource` when the row is built,
in `build_state_data.py` as a tripwire when the file is assembled, and again in
`CountyGuideImporter` on the way into the database. A redundant check costs
nothing. An address in the wrong hands cannot be taken back.

If the tripwire in `build_state_data.py` ever prints, the guardrail in
`ResourceIntake.kt` has a hole and that is the thing to fix — not the file.
