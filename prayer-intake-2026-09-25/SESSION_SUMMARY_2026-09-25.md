# Session summary: prayer tagging system and the rumination intake

**Session:** Claude Code cloud session, 2026-09-24 to 2026-09-25, working directory `paulbevins63-bit/ky-spiritual-guide-data`, branch `claude/gallant-cray-d8x4dz`.
**Files this summary sits beside:** `INTAKE_Rumination_Prayers_2026-09-25.md` (decision log), `new_prayer_records.json` (29 records), `similarity_report.json` (item-by-item scores).

---

## 1. What you asked for, in order

1. Confirm the session runs in the cloud.
2. Find the tagging system and category set used to load about 2,300 prayers into the Kentucky Spiritual Guide.
3. Assess how the tagging system works, from five uploaded files.
4. Confirm the plan for a new set of prayers against rumination, and for getting people up to act.
5. Run the new prayer file through a defined intake: similarity gate, evaluation, titles, tags, add, document.
6. Rule on variations: keep every modern rendering unless it repeats the same intent and wording as one already kept; flag new-to-new near-twins for merging together.
7. Commit and push (from the repository's stop hook).
8. This summary.

---

## 2. Where the prayer system lives

- **Not in this repository.** `ky-spiritual-guide-data` holds only county resource guides, one JSON per state, 271,078 resource rows in 13 categories. No prayer data has ever been committed to it.
- **Not on GitHub at all.** Your account has two repositories: this one and `policies` (the website).
- **On your PC**, in the app project: `C:/Users/paulb/AndroidStudioProjects/20260704_Claude_Spirit_App_rev2.6`, per your September 19 handoff document on Drive.
- **On Drive** I found only the surroundings: the user guides describing the finished shelves, the August 15 batch docs with the earlier 15-category scheme, June source snapshots that predate tagging, and the September 18 doc "Public Domain Morning Prayers: Stop, Get Up, and Act."

---

## 3. How the tagging system works (from the five files you uploaded)

**Files:** `Prayer_Category_Rubric_2026-08.md`, `prayers_catalog_v1.json` (catalog_version 200, 2,326 prayers), `PAUL_Shelf_Map_2026-09-03.md`, `prayer_vocabulary.json`, `Prayer_Categories_for_App.txt`, and later `station_prayers.json`.

- **One record, many shelves.** Each prayer is stored once with a `categories` array of slugs. A shelf is a saved view over that field, so a prayer with four categories shows on four shelves.
- **52 categories in eight groups**: The Daily Walk, Nightly Reflection, People, When It's Hard, Recovery, Hurting, Before God, The Church's Words. Six categories sit in two groups; still one tag, one shelf.
- **The rubric is the content gate.** Per category: what the prayer does for the one praying it, "Tag when," "Not when," "Distinct from," worked examples. Rule: tag what the prayer asks, not what words it contains. Two to six tags typical. Petition for Myself is the tag of last resort. Uncertain cases get a flag, not a guess.
- **The vocabulary drives search, not tagging.** Cores at 1.0, aliases at 0.5, whole-word, case-insensitive. Weights rank chips, never reject. Seventeen terms shared across categories on purpose.
- **Other layers:** `faiths` (14 values; "All" on 1,891 records), `voice` (i / we / neither, behind the "Praying alone" switch), `step_tag` (all twelve Steps curated; the tag pool is closed), Recovery Road stations built from pinned ids, then step tags, then categories, mutually exclusive.

**What the data confirmed:** catalog and vocabulary agree on all 52 slugs; zero flagged records; Petition for Myself alone on only 1 of 149; Liturgical Texts alone on only 3 of 390; co-tag pairs match the rubric's predictions.

**Soft spots I reported, which you said you are addressing:**

- Dire Need carries three long prayers (241, 118, 88 words) against its own "keep it small" rule.
- The Praying alone switch hides 1,218 of 2,326 prayers, including four of the 23 on Dire Need.
- Twenty records carry a plural `step_tags` list beside the single `step_tag`; confirm the app reads both.
- The shelf map is at v195 while the catalog is at v200.
- Only two prayers contain the word "craving" and neither is on Temptation & Craving.
- 77 single-tag prayers, mostly Adoration, Blessing, Surrender, Praise.

---

## 4. The intake rules you gave (2026-09-25)

1. Check each new prayer against all 2,326.
2. Exclude any new prayer 90 percent or more similar in text to one already in the catalog.
3. New prayers are exempt from the 90 percent test against each other, but not against the catalog.
4. After exclusions, read the survivors and judge whether they add value.
5. If they do: judge the modern title, say what needs changing, tag them, add them, document everything.
6. Most records hold one historic (Original tab) and one modern (Modern tab).
7. Credits: the proper original author, with Kentucky Bootleggers as "adapted by."
8. Keep every modern variation. Remove a variation only if it has the exact same intent and wording as prayers already kept.
9. Where two new prayers are too similar to each other, flag them; we merge them together rather than drop one.
10. Where to do the work: judgment here, insertion on the desktop, because the desktop has the id sequence, version bump conventions, shelf map generator, station and step wiring, and the project's CLAUDE.md.

---

## 5. What the intake produced

**Input:** `20260923224645_NEW_Prayers_to_Stop_Rumination.md`, 49 numbered items across 13 sources.

**Method:** word-level `difflib.SequenceMatcher(autojunk=False)` against every text field of every record, in two passes: raw, and with archaic forms and plural voice normalized (thee/thy → you/your, hast/-eth → modern, us/our/we → me/my/I). A first pass with the default matcher was thrown out because its junk heuristic fails on texts over 200 characters.

| Result | Count | Items |
|---|---:|---|
| Excluded by the 90 percent rule | 1 | 16, the 1914 Mullan Suscipe, 0.93 vs `historic_step11_010` |
| Same prayer already in the catalog; used only as an Original tab, not added | 5 | 1 and 2 (Aquinas), 13 (Nada te turbe), 25 (Prime collect), 37 (Newman "me" version, 0.85) |
| Dropped as having no content of their own | 3 | 6 (kept as item 3's `short_text`), 44 and 48 (pronoun-swapped copies of their historic texts) |
| Near-twin pairs flagged MERGE PENDING with a proposed merged text | 2 | 17 + 18 (Suscipe moderns), 35 + 36 (Southwell moderns) |
| Records ready to insert | 29 | 27 ready, 2 merge pending |

**Shelf impact:** The Circling Mind 30 → 43, all "i" voice. Guidance & Decisions +16, Morning +13, Peace +12, Being Useful +10. Mean 3.9 tags per record.

**Text fixes made and logged:** curly-quote typos, mixed voice ("grant us" in a "me" prayer, "Jesus, our God, You showed me"), a missing word ("walk straight the path"), one tangled line in the Anselm rendering, capitalization of You/Your to house style, "til" → "Till," and the "Take one slow breath. Stand, and do the next right thing" footer moved out of prayer text into the `footnote` field.

**Titles:** "Prayer:" prefixes and "(Modern English)" suffixes dropped throughout. Two renamed: item 15 to "When My Thoughts Circle," item 24 to "Clutter and Spin." One informal title kept and flagged: "Kicking the Panic Loop to the Curb."

**Credits as built:** `author` = original author; `attribution` = "After <author>. Adapted by Kentucky Bootleggers, 2026."; `original_attribution` = the translation citation, with any provenance doubt stated in plain words.

---

## 6. Provenance finding you need before anything ships

Six of the thirteen sources carry credits I could not stand behind. The Modern tabs can ship under the Kentucky Bootleggers credit regardless; the Original tabs and `author` fields should not name a saint the text may not belong to.

| Source | Status |
|---|---|
| "Bernard," Oratio Matutina | Wrong. It is the Gelasian Advent collect (`fellowship_prayers_0118`) plus an unsourced sentence. Credited to the Gelasian Sacramentary. |
| Ambrose, Splendor paternae gloriae | Hymn real; translator wrong. The English is Robert Bridges (1899), not Newman (1852). |
| de Sales, "Fear not what may happen tomorrow" | Author real; citation doubtful. From his letters, not the Introduction to the Devout Life. |
| de Sales, morning offering | Unverified. |
| "Anselm," Oratio ad Matutinum | Unverified. |
| "Southwell," morning resolution | Unverified. |
| "Coptic Agpeya, Third Hour" | Unverified; translator credit doubtful. |
| "Nerses Shnorhali" | Doubtful; does not match "I Confess with Faith." |

Recommendation: desktop Fable, with web access, verifies these before the Original tabs go live.

---

## 7. Open items for you

1. Approve or rewrite the two merged texts (both originals are printed in the log, section 5).
2. Decide the Suscipe record's Original tab: reuse `historic_step11_010`, ship single-tab with `see_also`, or override the rule for a tab that is not a new prayer. It is left null.
3. Decide whether the footer note stays on Circling Mind records, since the shelf header already says it.
4. Confirm "Kicking the Panic Loop to the Curb" belongs beside the Breviary on the same shelf.
5. Verify the six flagged credits.
6. Step tags are set on three records only (Aquinas 7 and 8 → step-11, Suscipe → step-3). Add to `station_prayers.json` if you want them on a step button.
7. Side finding: `man_of_god_0020` is Aquinas's Concede mihi opening but says author "Unknown."
8. Side finding: the catalog credits adaptations two ways (saint in `author` on `builtin21_09`; Kentucky Bootleggers in `author` on the 26 Adaptation records). These 29 follow your instruction: saint in `author`.

---

## 8. Desktop steps to ingest

1. Copy the three packet files into the app project.
2. Resolve section 7 items 1, 2, and 5.
3. Assign real ids in place of the `NEW_*` placeholders; strip or move the `_intake` block into `note`.
4. Insert into `prayers_catalog_v1.json`, bump `catalog_version` (201), write the `generated` note in the house style.
5. Re-run `tools/prayer_prepass/gen_shelf_map_and_index.py`.
6. Consider adding the old-corpus aliases to The Circling Mind in `prayer_vocabulary.json`: idle thoughts, wandering thoughts, vain thoughts, disquiet, distraction, brooding, get up, get moving.
7. Consider the 35 existing catalog prayers that use 1890s rumination language but are not on The Circling Mind (list available on request; examples: `forgiving_others_049` Underhill's "Prayer for Quieting Inner Turmoil," `tileston_prayers_0261` à Kempis, `tileston_prayers_0179` Arndt).

---

## 9. GitHub status

- The packet is committed locally as `c5a48da` on `claude/gallant-cray-d8x4dz`.
- Every push attempt returned HTTP 403: "Claude doesn't have GitHub access to paulbevins63-bit/ky-spiritual-guide-data for your organization." The GitHub API route returned 403 "Resource not accessible by integration." Reads work; writes do not.
- Likely cause: the Claude GitHub App installation does not include this repository with write access, or this session's credential predates the grant. Check https://github.com/apps/claude/installations/select_target. If the installation looks right, a fresh session will pick up the new credential.
- Nothing is on GitHub yet. The files were delivered as attachments in the conversation, so the desktop work does not depend on the push.
