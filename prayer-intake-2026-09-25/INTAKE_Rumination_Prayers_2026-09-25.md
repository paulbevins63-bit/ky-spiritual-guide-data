# Prayer intake: rumination and get-up prayers — decision log

**Date:** 2026-09-25. **Input:** `20260923224645_NEW_Prayers_to_Stop_Rumination.md` (49 numbered items in 13 source groups). **Checked against:** `prayers_catalog_v1.json`, catalog_version 200, 2,326 prayers, all three text fields (`full_text`, `original`, `modern`). **Rubric:** `Prayer_Category_Rubric_2026-08.md` (approved 2026-08-22), 52 categories. **Outputs:** `new_prayer_records.json` (29 records, catalog schema, placeholder ids), `similarity_report.json` (every item's best catalog match).

This packet was built in a cloud session that has the catalog but not the app project. Nothing was written to the catalog. The desktop step is: assign ids, confirm the two pending merges, decide the open items in §7, insert, bump `catalog_version`, and re-run `tools/prayer_prepass/gen_shelf_map_and_index.py`.
## 1. Method

1. **Parse.** Each numbered item became one candidate: its heading, its text, and any italic source or footer line. Original-language blocks (Latin, Coptic, Armenian) were kept as reference only; they are not prayers to add.
2. **Similarity gate.** Every item's text was compared with every text field of every catalog record, at word level, with `difflib.SequenceMatcher(autojunk=False)`. Two passes: raw, and with archaic forms and plural voice normalized (thee/thou/thy → you/your, hast/hath/-eth → modern, us/our/we → me/my/I) so that a pronoun swap or a thee-to-you edit cannot hide a duplicate. The score used is the higher of the two. **Exclusion threshold: 0.90.** A first pass with the default string matcher was discarded: its junk heuristic collapses on texts over 200 characters and scored Newman's "Support us" at 0.12 against its obvious twin.
3. **Within-file check.** Items were also compared with each other, but only to find near-twins for merging; per Paul's rule, file items are exempt from the 90% test against each other.
4. **Grouping.** The catalog stores one prayer as one record with an Original tab (`original`) and a Modern tab (`full_text`/`modern`). Each modern rendering became its own record carrying its source's historic translation as the Original tab. Repeating a historic text across records is existing practice (the Suscipe's Original tab appears in three records).
5. **Tagging.** By the rubric's "tag what it asks" rule, 2 to 6 categories per record, Petition for Myself never used, `the-circling-mind` only where the text names the loop, `morning` only where the text names the start of the day.
6. **Credits.** Per Paul (2026-09-25): `author` = the original author; `attribution` = "After <author>. Adapted by Kentucky Bootleggers, 2026."; `original_attribution` = the translation citation. Note that the catalog currently has two conventions (builtin21_09 puts the saint in `author`; the 26 Adaptation records put "Kentucky Bootleggers" in `author`). These records follow Paul's instruction; the desktop may want to normalize one way.
## 2. Excluded by the 90% rule

| Item | Heading | Score | Catalog twin |
|---|---|---:|---|
| 16 | Original: Morning Surrender of Mind and Will (1914 Mullan) | 0.93 | `historic_step11_010` Suscipe - Ignatius Loyola |

That is the only exclusion. Every other item scores under 0.90; the full table is in `similarity_report.json`.

## 3. Passes the number, fails the spirit — Paul's call

These pass the 90% test but are the same prayer already in the catalog, in another translation or voice. I did **not** add them as new prayers. Where a modern rendering depends on them, they serve as the Original tab (or the catalog's own text does).

| Item | What it is | Score | Already in catalog as | What I did |
|---|---|---:|---|---|
| 37 | Newman "Support me, O Lord" | 0.85 (voice-normalized) | `historic_step12_025`, "us" voice | Not added. Items 37's text is the historic prayer with pronouns changed; the Original tab of the two Newman records uses the true historic "us" wording. |
| 25 | Prime collect, Bute 1879 translation | 0.66 | `tileston_prayers_0408`, BCP wording | Used only as the Original tab of the two Prime records, with `see_also` to Tileston's. |
| 40 | "Stir up" credited to St. Bernard | 0.55 | `fellowship_prayers_0118`, Gelasian Advent collect | The Latin and the English are the Advent collect plus one added sentence with no source. Original tab = the collect, credited to the Gelasian Sacramentary; the added petition lives in the Modern tabs under the Kentucky Bootleggers credit. |
| 13 | Nada te turbe, Dalton 1860 | 0.39 | `forgiving_others_044`, `courage_010` (modern wording) | Used as the Original tab of the two Teresa records, `see_also` both. |
| 1, 2 | Aquinas "Concede mihi", 1890 and 1885 | 0.38 | `man_of_god_0020` carries the opening sentence only, author "Unknown" | Used as Original tabs. Side finding: `man_of_god_0020` is Aquinas and should say so, with `see_also` to these. |

## 4. Dropped as having no content of their own

Per Paul's rule, a modern is removed only when it has the same intent **and** the same wording as something kept.

| Item | Heading | Twin | Reason |
|---|---|---|---|
| 6 | God, Let Me Do What Pleases You | item 3 | A cut-down of item 3's lines (56% overlap, same sentences). **Kept as item 3's `short_text`** rather than thrown away, which is how the catalog handles a short form (5 records use the field). |
| 44 | Enkindle Our Hearts With Your True Light (second copy) | item 43 | Item 43 with pronouns modernized and the same title (79% overlap). Item 43 is the Original tab; 45 and 46 are the moderns. |
| 48 | Make Me Worthy To Do Your Will Today | item 47 | Item 47 with "Thee" → "You" and one added closing line (80% overlap). Item 47 is the Original tab; 49 is the modern. If you want a faithful-modern record for the Armenian prayer, 48 is it. |

## 5. Near-twins to merge together — MERGE PENDING

Both pairs are one prayer written twice. I proposed a merged text in the record and marked it `MERGE PENDING`; the two originals are printed here so you can redo it your way.
### Items 17 + 18 → `NEW_suscipe_for_rumination` — "I Surrender My Worries to You"

**Item 17: Prayer: Take All That I Am - A Suscipe for Rumination (2026 Modern English):**

> Lord, take everything that makes me who I am—my freedom, my memory, my intellect, and my decisions. Everything I have was given to me by you. I give it back today and ask you to guide it. Just give me your love and your steady presence. That is more than enough for me, and I will not let other worries consume my mind.

**Item 18: Prayer: I Surrender Worries to You (2026 Modern English):**

> Lord, take everything that makes me who I am:
> my freedom, my memory, my intellect, and my will.
> You have given all of it to me Lord, I return it now.
> All is Yours - do with it what You will.
> Give me only Your love and Your presence.
> That is more than enough for me.
> I will not let other worries consume my mind
> As I walk in the path You have set before me.
> Through Christ our Lord. Amen.

**Proposed merge (in the record):**

> Lord, take everything that makes me who I am:
> my freedom, my memory, my intellect, and my will.
> You have given all of it to me, Lord; I return it now.
> All is Yours. Do with it what You will.
> Give me only Your love and Your steady presence.
> That is more than enough for me,
> and I will not let other worries consume my mind
> as I walk the path You have set before me.
> Through Christ our Lord. Amen.
### Items 35 + 36 → `NEW_southwell_i_give_you_this_day` — "I Give You This Day"

**Item 35: Let's Get To Work (Modern English)**

> Jesus Christ, source of all goodness, thank You for bringing me safely through the night. I offer You everything I do today. Please guide my hands, watch over my words, and take charge of my heart so I don't waste time on things that go against Your will. Let's get to work. Amen.

**Item 36: I Give You This Day (Modern English)**

> Jesus, source of everything good,
> Thank You for getting me through the night.
> I give You this day - everything I do.
> Guide my hands, guard my mouth, and rule my heart,
> so I don't do anything against Your will today.
> Guide me now to the next right step.
> Amen.

**Proposed merge (in the record):**

> Jesus Christ, source of everything good,
> thank You for bringing me safely through the night.
> I give You this day, everything I do.
> Guide my hands, guard my mouth, and rule my heart,
> so I don't waste today on anything against Your will.
> Guide me now to the next right step.
> Let's get to work. Amen.
## 6. Provenance — the credits I could and could not stand behind

Paul's instruction is that every record names the proper original author. For five of the thirteen sources I cannot confirm the attribution the file gives, and for three the file's credit is wrong. The Modern tabs can ship under the Kentucky Bootleggers credit either way; the Original tab and the `author` field should not name a saint the text may not belong to. Each record's `original_attribution` states the doubt in plain words so nothing ships silently.

| Source | Status | Detail |
|---|---|---|
| Aquinas, *Concede mihi* | Confirmed prayer | The catalog's `man_of_god_0020` is its opening. The 1890 and 1885 translation citations are plausible, not page-checked. |
| Roman Breviary, *Aperi, Domine* | Confirmed prayer | Standard prayer before the Office; 1914 Desclée translation plausible. |
| Teresa, *Nada te turbe* | Confirmed | Dalton did translate Teresa in the 1850s–60s. |
| Ignatius, Suscipe | Confirmed | Excluded as a new text; see §2. |
| de Sales, "Fear not what may happen tomorrow" | Author confirmed, **citation doubtful** | This is a genuine de Sales passage but from his letters, not the *Introduction to the Devout Life*; the French supplied is a modern paraphrase. Fix the citation. |
| de Sales, morning offering "empty anxieties" | **Unverified** | Could not be matched to the *Introduction*; the French is not seventeenth-century. |
| Gelasian collect at Prime | Confirmed | In the catalog as `tileston_prayers_0408`. Bute's 1879 Breviary translation is real. |
| Ambrose, *Splendor paternae gloriae* | Hymn confirmed, **translator wrong** | "O Splendor of God's glory bright" is Robert Bridges (Yattendon Hymnal, 1899), not Newman 1852. Public domain either way; correct the credit. |
| "Anselm", *Oratio ad Matutinum* | **Unverified** | The Latin is not recognizable as Anselm's *Orationes*. |
| "Southwell", morning resolution | **Unverified** | No source found; Southwell did write prayers, so it may be real. |
| Newman, "Support us" | Confirmed | In the catalog as `historic_step12_025`; from the 1843 sermon "Wisdom and Innocence". |
| "Bernard", *Oratio Matutina* | **Credit wrong** | It is the Gelasian Advent collect (`fellowship_prayers_0118`) plus an unsourced sentence. Credited here to the Gelasian Sacramentary. |
| "Coptic Agpeya, Third Hour" | **Unverified** | Not matched to the Agpeya; Marcus Simaika was a museum founder, not a known translator of it. |
| "Nerses Shnorhali", morning prayer | **Doubtful** | Does not match the known verses of *I Confess with Faith*. |

Recommendation: desktop Fable, with web access, verifies the six flagged sources before the Original tabs ship. Where a source cannot be verified, either credit the Modern tab as a Kentucky Bootleggers original "in the manner of" the tradition and drop the Original tab, or hold the record.

## 7. Open items for Paul

1. **Two merges** in §5 need your approval or your own wording.
2. **The Suscipe record has no Original tab.** Item 16 is excluded at 93%. Options: (a) reuse `historic_step11_010`'s text as the Original tab, (b) ship single-tab with `see_also`, (c) override the rule for a tab that is not a new prayer. I left `original` null.
3. **Footer lines.** Eight items end with "Take one slow breath. Stand, and do the next right thing." I moved it to the `footnote` field on those records. The Circling Mind shelf already carries a header saying the same thing, so you may prefer to drop the footnote on shelf records and keep it only where the prayer is not on that shelf.
4. **Register.** Item 21 ("guts", "has my back", "kick ... to the curb") is the one informal prayer in a batch of Breviary and Aquinas. It earns its place on the shelf; confirm you want it beside them.
5. **Six unverified credits** in §6.
6. **Step tags** are set on three records only (Aquinas 7 and 8 → step-11, Suscipe → step-3). Every step is curated, so a step tag alone puts nothing on a step button; add to `station_prayers.json` if you want them there.
7. **Side finding:** `man_of_god_0020` has author "Unknown" and is Aquinas's *Concede mihi* opening. Worth fixing while these go in.
8. **Side finding:** the catalog's credit conventions differ (`author` = saint on `builtin21_09`; `author` = Kentucky Bootleggers on the 26 Adaptation records). These records follow your 2026-09-25 instruction: saint in `author`, "Adapted by Kentucky Bootleggers" in `attribution`.

## 8. Shelf impact

| Shelf | Added | Now (v200 + these) |
|---|---:|---:|
| guidance-and-decisions | 16 | 185 |
| morning | 13 | 141 |
| the-circling-mind | 13 | 43 |
| peace | 12 | 217 |
| being-useful | 10 | 86 |
| trust-and-faith | 9 | 307 |
| fear-and-worry | 8 | 78 |
| wisdom-and-clarity | 6 | 186 |
| dedication-and-service | 5 | 239 |
| evening-and-night | 5 | 201 |
| presence | 4 | 90 |
| surrender | 4 | 227 |
| strength-and-endurance | 4 | 152 |
| praise | 4 | 194 |
| temptation-and-craving | 3 | 97 |
| protection | 2 | 198 |
| gratitude-and-thanksgiving | 2 | 179 |
| humility | 1 | 206 |
| liturgical-texts | 1 | 391 |
| patience | 1 | 83 |
| letting-go | 1 | 41 |
| comfort | 1 | 99 |
| courage | 1 | 74 |

29 records, 126 tags, mean 4.34 per record. The Circling Mind goes from 30 to 43; every new record on it is 'i' voice. Word counts run 30–90; the shortest (Light Up My Heart, 30 words) will sit at the top of the shortest-first shelves it is on.

## 9. Record-by-record

Each entry: the title decision, the text fixes, the tags with the reason, and the credit lines as they will appear.
### 1. `NEW_aquinas_clarity_direction` — Morning Prayer for Clarity and Direction

- **From:** item(s) 3 of the file; heading was "Morning Prayer for Clarity and Direction (Modern English)".
- **Status:** ready
- **Title:** Kept. Dropped the "(Modern English)" suffix, as on every record below.
- **Why it earns a place:** Full-length morning rendering; orders the day and the mind. Not tagged the-circling-mind: it asks for order, not for the loop to stop.
- **Categories:** `morning`, `guidance-and-decisions`, `wisdom-and-clarity`, `peace`; **voice:** i; **faiths:** All, Catholic, Anglican, Recovery; **words:** 87
- **Text fixes:** "Keep me steady Lord." -> "Keep me steady, Lord." Item 6 ("God, Let Me Do What Pleases You") folded in as short_text: it is a cut-down of this text with the same lines and intent (56% word overlap), not a separate prayer.
- **short_text:** "God, put Your desires in my heart. Help me think clearly, see truthfully, and do what plea…"
- **author:** St. Thomas Aquinas
- **attribution:** After St. Thomas Aquinas (1225–1274), Concede mihi, misericors Deus. Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Concede mihi, misericors Deus (Prayer for Ordering One's Life)
- **original_attribution:** Attributed to St. Thomas Aquinas (1225–1274). English translation as printed in The Roman Missal Adapted for the Laity (New York, 1890). Public domain.
- **provenance:** confirmed prayer; translation citation plausible, unverified

**Modern tab:**

> Heavenly Father,
> Put Your desires in my heart.
> Help me think clearly, see truthfully, and do what pleases You.
> Put my thoughts, life, and schedule in proper order for Your glory.
> Lord, make it clear to me what You want today,
> And give me the guidance and strength to do it well.
> 
> Keep me steady, Lord.
> Not shaken by hard times, and not inflated by good ones.
> Let me be moved only by what draws me to You
> or warns me I'm drifting away from You. Amen.

### 2. `NEW_aquinas_desire_what_pleases` — Let Me Desire What Pleases You

- **From:** item(s) 4 of the file; heading was "Prayer: Let Me Desire What Pleases You (Modern English)".
- **Status:** ready
- **Title:** Kept; dropped the "Prayer:" prefix (catalog titles do not carry it).
- **Why it earns a place:** The formal register; follows the 1885 wording line by line. Paired with the 1885 Original tab because it derives from it.
- **Categories:** `guidance-and-decisions`, `wisdom-and-clarity`, `dedication-and-service`, `peace`; **voice:** i; **faiths:** All, Catholic, Anglican, Recovery; **words:** 90
- **Text fixes:** Curly-quote typo: Don”t -> Don't
- **author:** St. Thomas Aquinas
- **attribution:** After St. Thomas Aquinas (1225–1274), Concede mihi, misericors Deus. Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Concede mihi, misericors Deus (Prayer to Order Life Wisely)
- **original_attribution:** Attributed to St. Thomas Aquinas (1225–1274). English translation as printed in The Key of Heaven (Burns & Oates, London, 1885). Public domain.
- **provenance:** confirmed prayer; translation citation plausible, unverified

**Modern tab:**

> Grant me, merciful God, the desire to eagerly do what pleases You,
> To examine wisely, to acknowledge truthfully, and to accomplish perfectly,
> for the praise and glory of Your name.
> 
> Order my life, Lord God! Show me what You want me to do.
> Help me do it as I should, in a way that is fitting and good for my soul.
> 
> Don't let me get proud in good times, nor disheartened in bad ones.
> Let me be happy only doing Your will, and be dispirited only when separated from You.

### 3. `NEW_aquinas_moved_only_by_you` — Let Me Be Moved Only by You

- **From:** item(s) 5 of the file; heading was "Prayer: Let Me Be Moved Only by You (Modern English)".
- **Status:** ready
- **Title:** Kept.
- **Why it earns a place:** 36 words; the whole prayer compressed to one breath. Different verbiage from every other Aquinas rendering, so it stays.
- **Categories:** `guidance-and-decisions`, `peace`, `humility`; **voice:** i; **faiths:** All, Catholic, Anglican, Recovery; **words:** 36
- **author:** St. Thomas Aquinas
- **attribution:** After St. Thomas Aquinas (1225–1274), Concede mihi, misericors Deus. Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Concede mihi, misericors Deus (Prayer for Ordering One's Life)
- **original_attribution:** Attributed to St. Thomas Aquinas (1225–1274). English translation as printed in The Roman Missal Adapted for the Laity (New York, 1890). Public domain.
- **provenance:** confirmed prayer; translation citation plausible, unverified

**Modern tab:**

> Loving Father, teach me to love what You love, know what You want, and do it well. Keep me steady in hardship and humble in success, moved only by what brings me closer to You. Amen.

### 4. `NEW_aquinas_quiet_my_circling_mind` — Quiet My Circling Mind

- **From:** item(s) 7 of the file; heading was "Prayer: Quiet My Circling Mind (Modern English)".
- **Status:** ready
- **Title:** Kept. This is the shelf-defining title of the batch.
- **Why it earns a place:** The rumination rendering: names the replay, asks for the present, the order, the next thing. Step 11 eligible (conscious contact, guidance).
- **Categories:** `the-circling-mind`, `presence`, `guidance-and-decisions`, `peace`; **step:** step-11; **voice:** i; **faiths:** All, Catholic, Anglican, Recovery; **words:** 50
- **author:** St. Thomas Aquinas
- **attribution:** After St. Thomas Aquinas (1225–1274), Concede mihi, misericors Deus. Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Concede mihi, misericors Deus (Prayer for Ordering One's Life)
- **original_attribution:** Attributed to St. Thomas Aquinas (1225–1274). English translation as printed in The Roman Missal Adapted for the Laity (New York, 1890). Public domain.
- **provenance:** confirmed prayer; translation citation plausible, unverified

**Modern tab:**

> Gracious God, quiet my circling mind.
> I keep replaying the same thoughts and getting nowhere.
> Bring my thinking back into the present,
> Put my day before me in clear, simple order.
> Show me the next right thing You have for me,
> And settle me enough, Lord, to do it. Amen.

### 5. `NEW_aquinas_i_dont_need_to_see_everything` — I Don't Need to See Everything

- **From:** item(s) 8 of the file; heading was "Prayer: I Don”t Need To See Everything (Modern English)".
- **Status:** ready
- **Title:** Fixed the typo; otherwise kept.
- **Why it earns a place:** "My mind wants to run ahead and around in circles" is the loop; "just show me what is mine to do" is the way out. Surrender for "The rest is up to You."
- **Categories:** `the-circling-mind`, `guidance-and-decisions`, `trust-and-faith`, `surrender`, `being-useful`; **step:** step-11; **voice:** i; **faiths:** All, Catholic, Anglican, Recovery; **words:** 65
- **Text fixes:** Curly-quote typo: don”t -> don't (text and title)
- **author:** St. Thomas Aquinas
- **attribution:** After St. Thomas Aquinas (1225–1274), Concede mihi, misericors Deus. Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Concede mihi, misericors Deus (Prayer for Ordering One's Life)
- **original_attribution:** Attributed to St. Thomas Aquinas (1225–1274). English translation as printed in The Roman Missal Adapted for the Laity (New York, 1890). Public domain.
- **provenance:** confirmed prayer; translation citation plausible, unverified

**Modern tab:**

> Father, put my life in order to honor You.
> My mind wants to run ahead and around in circles.
> Make the path plain.
> I don't need to see everything, God.
> Just show me what is mine to do and give me grace to do it.
> With your help God, I will do the work You've set before me.
> The rest is up to You. Amen.

### 6. `NEW_aquinas_steady_hands` — Steady Hands and a Steady Heart

- **From:** item(s) 9 of the file; heading was "Prayer: For Steady Hands and a Steady Heart (Modern English)".
- **Status:** ready
- **Title:** Dropped "Prayer: For"; kept the phrase.
- **Why it earns a place:** Renders the "not troubled in adversity, not puffed up in prosperity" half that the other moderns skip. "Keep me busy at what You have given me" is Being Useful.
- **Categories:** `peace`, `strength-and-endurance`, `being-useful`; **voice:** i; **faiths:** All, Catholic, Anglican, Recovery; **words:** 59
- **author:** St. Thomas Aquinas
- **attribution:** After St. Thomas Aquinas (1225–1274), Concede mihi, misericors Deus. Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Concede mihi, misericors Deus (Prayer for Ordering One's Life)
- **original_attribution:** Attributed to St. Thomas Aquinas (1225–1274). English translation as printed in The Roman Missal Adapted for the Laity (New York, 1890). Public domain.
- **provenance:** confirmed prayer; translation citation plausible, unverified

**Modern tab:**

> Lord, give me steady hands and a steady heart.
> Don't let me unravel when times get hard.
> Don't let me get distracted when times are good.
> Don't let my highs and lows decide the day.
> Let me draw closer to You, God.
> Keep me level, keep me close, and
> Keep me busy at what You have given me.
> Amen.

### 7. `NEW_aperi_open_my_mouth` — Open My Mouth to Bless Your Name

- **From:** item(s) 11 of the file; heading was "_==Faithful modern: Prayer Before the Divine Office - Open My Mouth==_".
- **Status:** ready
- **Title:** Took the title from the file's item 10 heading; dropped "Faithful modern:" and the == markers.
- **Why it earns a place:** Faithful rendering; "vain, evil, and wandering thoughts" is the 1890s word for the loop. Liturgical Texts added because the Original tab is a rite-slot prayer (before the Office), per the 2026-08-21 ruling.
- **Categories:** `the-circling-mind`, `wisdom-and-clarity`, `praise`, `liturgical-texts`; **voice:** i; **faiths:** All, Catholic, Recovery; **words:** 55
- **Text fixes:** Italic footer line moved out of the prayer text into the footnote field.
- **footnote:** "Take one slow breath. Stand, and do the next right thing: ta…"
- **author:** Traditional — Roman Breviary
- **attribution:** After the Roman Breviary prayer before the Divine Office (Aperi, Domine). Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Aperi, Domine, os meum (Prayer Before the Divine Office)
- **original_attribution:** Traditional — Roman Breviary, prayer before the Divine Office. English translation from The Roman Breviary in English (Desclée, Tournai, 1914). Public domain.
- **provenance:** confirmed prayer (Roman Breviary); translation citation plausible

**Modern tab:**

> Lord, open my mouth to bless Your holy name.
> Cleanse my heart of all vain, evil, and wandering thoughts.
> Enlighten my mind and set my heart on fire with love for You.
> Let me pray this worthily, attentively, and devoutly,
> and be heard in the presence of Your divine majesty.
> Through Christ our Lord. Amen.

### 8. `NEW_aperi_pray_and_do` — A Prayer to Pray and Do

- **From:** item(s) 12 of the file; heading was "_==A Prayer to Pray and Do:==_".
- **Status:** ready
- **Title:** Kept.
- **Why it earns a place:** Free adaptation that turns the pre-Office prayer into a get-up prayer ("Move my hands to do Your will"). Different enough from item 11 (52% overlap) to stand on its own.
- **Categories:** `the-circling-mind`, `praise`, `being-useful`; **voice:** i; **faiths:** All, Catholic, Recovery; **words:** 45
- **Text fixes:** Footer line moved to footnote. Item was labelled historic by its heading but is a free modern adaptation; treated as Modern.
- **footnote:** "Take one slow breath. Stand, and do the next right thing: ta…"
- **author:** Traditional — Roman Breviary
- **attribution:** After the Roman Breviary prayer before the Divine Office (Aperi, Domine). Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Aperi, Domine, os meum (Prayer Before the Divine Office)
- **original_attribution:** Traditional — Roman Breviary, prayer before the Divine Office. English translation from The Roman Breviary in English (Desclée, Tournai, 1914). Public domain.
- **provenance:** confirmed prayer (Roman Breviary); translation citation plausible

**Modern tab:**

> Lord, let me bless Your name.
> Cleanse my heart and quiet my mind.
> Set my heart on fire, God.
> Let me pray to You fervently and devoutly.
> Hear my cries to you Jesus!
> Move my hands to do Your will.
> Through Christ our Lord. Amen.

### 9. `NEW_teresa_you_alone_are_enough` — You Alone Are Enough

- **From:** item(s) 14 of the file; heading was "Prayer For Peace Tonight: You Alone Are Enough (2026 Modern English):".
- **Status:** ready
- **Title:** Shortened from "Prayer For Peace Tonight: You Alone Are Enough"; the shelf and the Evening tag already say "tonight".
- **Why it earns a place:** Nada te turbe re-aimed at bedtime with a next-right-thing close. The two existing Teresa records are the bare text; this is an adaptation, so it is new content. see_also links both.
- **Categories:** `evening-and-night`, `peace`, `trust-and-faith`, `patience`, `fear-and-worry`, `guidance-and-decisions`; **voice:** i; **faiths:** All, Catholic, Anglican, Recovery; **words:** 53
- **Text fixes:** "Lord, Don't" -> "Lord, don't" Footer line moved to footnote.
- **see_also:** `forgiving_others_044`, `courage_010`
- **footnote:** "Take one slow breath. Stand, and do the next right thing.…"
- **author:** St. Teresa of Ávila
- **attribution:** After St. Teresa of Ávila (1515–1582), Nada te turbe. Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Nada te turbe (Let Nothing Disturb Thee)
- **original_attribution:** St. Teresa of Ávila (1515–1582). English translation by John Dalton (London, 1860). Public domain. An alternate 1904 translation by David Lewis exists; the two existing catalog records (courage_010, forgiving_others_044) carry modern-English wording.
- **provenance:** confirmed prayer; Dalton 1860 plausible

**Modern tab:**

> Lord, don't let anything disturb or frighten me tonight.
> While everything else changes, You are eternal.
> Give me the patience to bear my burdens.
> You are all I need - You alone are enough.
> Give me the guidance to do the next right thing.
> I pray this through Jesus Christ our Lord. Amen.

### 10. `NEW_teresa_when_my_thoughts_circle` — When My Thoughts Circle

- **From:** item(s) 15 of the file; heading was "Prayer For Peace Tonight: Quiet My Mind Lord, You are Enough (2026 Modern English):".
- **Status:** ready
- **Title:** Renamed from "Prayer For Peace Tonight: Quiet My Mind Lord, You are Enough" (two colons deep, and "Quiet My Mind" collides with the Aquinas title). Runner-up: "Quiet My Mind in This Moment".
- **Why it earns a place:** The rumination rendering of Nada te turbe: "when my thoughts circle and frighten me, steady me."
- **Categories:** `the-circling-mind`, `evening-and-night`, `fear-and-worry`, `peace`, `trust-and-faith`, `guidance-and-decisions`; **voice:** i; **faiths:** All, Catholic, Anglican, Recovery; **words:** 47
- **see_also:** `forgiving_others_044`, `courage_010`
- **author:** St. Teresa of Ávila
- **attribution:** After St. Teresa of Ávila (1515–1582), Nada te turbe. Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Nada te turbe (Let Nothing Disturb Thee)
- **original_attribution:** St. Teresa of Ávila (1515–1582). English translation by John Dalton (London, 1860). Public domain. An alternate 1904 translation by David Lewis exists; the two existing catalog records (courage_010, forgiving_others_044) carry modern-English wording.
- **provenance:** confirmed prayer; Dalton 1860 plausible

**Modern tab:**

> Father, when my thoughts circle and frighten me, steady me.
> When all else passes around me, You do not change.
> Quiet my mind in this moment.
> Guide me to do Your will.
> You are all I need, You alone are enough.
> Through Christ Jesus, our Lord. Amen.

### 11. `NEW_suscipe_for_rumination` — I Surrender My Worries to You

- **From:** item(s) 17, 18 of the file; heading was "Prayer: I Surrender Worries to You (2026 Modern English):".
- **Status:** MERGE PENDING — Paul to approve the merged text
- **Title:** From item 18 with "Worries" made possessive; item 17's "Take All That I Am - A Suscipe for Rumination" is a good subtitle but too long for a shelf.
- **Why it earns a place:** The Suscipe already sits on Steps 3 and 11 in three records; what is new here is the last two lines, which turn it against rumination. Item 16 (the 1914 Mullan translation) is EXCLUDED by the 90% rule (93% against historic_step11_010), so this record has NO Original tab of its own: `original` is left for the desktop to fill from builtin21_09 or historic_step11_010, or the record ships single-tab with see_also. Your call.
- **Categories:** `surrender`, `the-circling-mind`, `letting-go`, `trust-and-faith`, `dedication-and-service`; **step:** step-3; **voice:** i; **faiths:** All, Catholic, Recovery; **words:** 81
- **Text fixes:** Items 17 and 18 are one prayer twice (62% overlap, same intent, same lines). Proposed merge keeps 18's line structure and closing, and takes 17's "steady presence". Both source texts are printed in the log for you to redo the merge your way.
- **see_also:** `builtin21_09`, `historic_step11_010`, `historic_step3_001`
- **author:** St. Ignatius of Loyola
- **attribution:** After St. Ignatius of Loyola (1491–1556), the Suscipe. Adapted by Kentucky Bootleggers, 2026.
- **original_title:** None
- **original_attribution:** St. Ignatius of Loyola, the Suscipe (Spiritual Exercises, 1548). The 1914 Mullan translation supplied with this batch duplicates historic_step11_010 at 93% and is not added; reuse that record's text or builtin21_09's Original tab here.
- **provenance:** confirmed; Original tab excluded by the 90% rule

**Modern tab:**

> Lord, take everything that makes me who I am:
> my freedom, my memory, my intellect, and my will.
> You have given all of it to me, Lord; I return it now.
> All is Yours. Do with it what You will.
> Give me only Your love and Your steady presence.
> That is more than enough for me,
> and I will not let other worries consume my mind
> as I walk the path You have set before me.
> Through Christ our Lord. Amen.

### 12. `NEW_desales_i_will_not_fear_tomorrow` — I Will Not Fear Tomorrow

- **From:** item(s) 20 of the file; heading was "I Will Not Fear Tomorrow - After St. Francis de Sales (Modern English)".
- **Status:** ready
- **Title:** Dropped "- After St. Francis de Sales"; the credit line carries that.
- **Why it earns a place:** Faithful first-person rendering of the "fear not tomorrow" passage with one added line of resolve. "Cast every fearful possibility from my heart" is the loop.
- **Categories:** `fear-and-worry`, `trust-and-faith`, `peace`, `the-circling-mind`, `strength-and-endurance`; **voice:** i; **faiths:** All, Catholic, Anglican, Recovery; **words:** 90
- **Text fixes:** Footer line moved to footnote.
- **footnote:** "Take one slow breath. Stand, and do the next right thing.…"
- **author:** St. Francis de Sales
- **attribution:** After St. Francis de Sales (1567–1622). Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Fear Not What May Happen Tomorrow
- **original_attribution:** St. Francis de Sales (1567–1622). English text as printed 1900 (credited in the source file to W. J. Knox Little, London). Public domain. PROVENANCE: the passage is genuine de Sales but comes from his letters, not the Introduction to the Devout Life; verify the citation before publishing it.
- **provenance:** confirmed author; cited source doubtful (letters, not the Introduction)

**Modern tab:**

> Lord, I will not fear what may happen tomorrow.
> You are the same eternal Father who cares for me today,
> and You will care for me tomorrow and every day of my life.
> Either You will keep me from suffering,
> or You will give me strength and faith to bear it.
> So I will be at peace, and I will put away all anxious thoughts,
> and cast every fearful possibility from my heart.
> I will walk in Your ways today and all my days.
> In Jesus name, I pray, Amen.

### 13. `NEW_desales_panic_loop` — Kicking the Panic Loop to the Curb

- **From:** item(s) 21 of the file; heading was "Kicking the Panic Loop To The Curb (Modern English)".
- **Status:** ready
- **Title:** Kept; the only informal title in the batch and it earns it. Make sure you want "guts" and "has my back" on the same shelf as the Breviary.
- **Why it earns a place:** The informal register of the same passage; "panic loop" names rumination in the reader's own words. Evening & Night for "tonight".
- **Categories:** `fear-and-worry`, `the-circling-mind`, `trust-and-faith`, `peace`, `evening-and-night`; **voice:** i; **faiths:** All, Catholic, Anglican, Recovery; **words:** 84
- **Text fixes:** Footer line moved to footnote. Title case: "To The" -> "to the".
- **footnote:** "Take one slow breath. Stand, and do the next right thing.…"
- **author:** St. Francis de Sales
- **attribution:** After St. Francis de Sales (1567–1622). Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Fear Not What May Happen Tomorrow
- **original_attribution:** St. Francis de Sales (1567–1622). English text as printed 1900 (credited in the source file to W. J. Knox Little, London). Public domain. PROVENANCE: the passage is genuine de Sales but comes from his letters, not the Introduction to the Devout Life; verify the citation before publishing it.
- **provenance:** confirmed author; cited source doubtful (letters, not the Introduction)

**Modern tab:**

> Lord, I'm going to stop stressing over what might go wrong tomorrow.
> You're the same God who has my back tonight,
> and You're going to take care of me tomorrow and every single day after that.
> Lord, I know You'll either keep the trouble away from me,
> or You'll give me the guts and resilience to handle it.
> I'm going to settle down inside now, relax,
> and kick every anxious thought and panic loop straight out of my head.
> Thank You, Jesus Christ. Amen.

### 14. `NEW_desales_next_right_step` — Guide Me Now to the Next Right Step

- **From:** item(s) 23 of the file; heading was "Prayer: Guide Me Now to the Next Right Step (Modern English - 2026):".
- **Status:** ready
- **Title:** Dropped "Prayer:" prefix.
- **Why it earns a place:** Morning offering with the worries handed over. Provenance of the Original tab is unverified (see the log); the Modern tab can ship under the Kentucky Bootleggers credit regardless.
- **Categories:** `morning`, `surrender`, `trust-and-faith`, `guidance-and-decisions`, `fear-and-worry`; **voice:** i; **faiths:** All, Catholic, Anglican, Recovery; **words:** 72
- **Text fixes:** Footer line moved to footnote.
- **footnote:** "Take one slow breath. Stand, and do the next right thing.…"
- **author:** St. Francis de Sales
- **attribution:** After St. Francis de Sales (1567–1622). Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Morning Offering Against Empty Anxieties
- **original_attribution:** Credited in the source file to St. Francis de Sales, Introduction to the Devout Life (Rivingtons, London, 1888). PROVENANCE UNVERIFIED: the French given is not seventeenth-century text and the passage could not be matched to the Introduction. Verify before crediting de Sales.
- **provenance:** UNVERIFIED

**Modern tab:**

> God, today I'm giving You all my thoughts, words and actions.
> Take away the needless worries troubling me.
> With Your divine guidance, order all that happens to me today.
> Let my mind rest and my heart be at ease in Your will.
> I trust You to turn all that happens to Your glory and my good.
> Guide me now to the next right step on Your path.
> Through Christ our Lord. Amen.

### 15. `NEW_desales_clutter_and_spin` — Clutter and Spin

- **From:** item(s) 24 of the file; heading was "For Clutter and Spin (Modern English - 2026):".
- **Status:** ready
- **Title:** Dropped "For"; "Clutter and Spin" is a better shelf title than "For Clutter and Spin".
- **Why it earns a place:** "Pointless anxieties that clutter and spin inside my head" is the most exact description of rumination in the batch.
- **Categories:** `morning`, `surrender`, `trust-and-faith`, `the-circling-mind`, `fear-and-worry`, `peace`; **voice:** i; **faiths:** All, Catholic, Anglican, Recovery; **words:** 57
- **Text fixes:** Capitalized You/Your to match the catalog's house style (every other modern in this file capitalizes).
- **author:** St. Francis de Sales
- **attribution:** After St. Francis de Sales (1567–1622). Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Morning Offering Against Empty Anxieties
- **original_attribution:** Credited in the source file to St. Francis de Sales, Introduction to the Devout Life (Rivingtons, London, 1888). PROVENANCE UNVERIFIED: the French given is not seventeenth-century text and the passage could not be matched to the Introduction. Verify before crediting de Sales.
- **provenance:** UNVERIFIED

**Modern tab:**

> God, I give You this day: everything I think, everything I say, and everything I do. Push away the pointless anxieties that clutter and spin inside my head. Take charge of whatever happens to me today, and help my heart stay calm and quiet in Your plan, knowing You will look out for me through every moment.

### 16. `NEW_prime_strong_shield` — Be My Strong Shield for Today

- **From:** item(s) 26 of the file; heading was "Be My Strong Shield For Today (Modern English)".
- **Status:** ready
- **Title:** Kept; title case fixed ("For" -> "for").
- **Why it earns a place:** "Take the steering wheel of my mind so my thoughts aren't spinning out in fear" earns the shelf; "bad habits" earns Temptation & Craving.
- **Categories:** `morning`, `protection`, `the-circling-mind`, `fear-and-worry`, `temptation-and-craving`, `gratitude-and-thanksgiving`; **voice:** i; **faiths:** All, Catholic, Anglican, Recovery; **words:** 71
- **see_also:** `tileston_prayers_0408`
- **author:** Traditional — Gelasian Sacramentary
- **attribution:** After the Gelasian collect at Prime (Domine Deus omnipotens). Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Domine Deus omnipotens (Collect at Prime)
- **original_attribution:** Traditional — Gelasian Sacramentary, the collect at Prime. English translation by John, Marquess of Bute, The Roman Breviary (Edinburgh, 1879). Public domain. The same collect is already in the catalog in the Book of Common Prayer wording as tileston_prayers_0408.
- **provenance:** confirmed (in catalog as tileston_prayers_0408)

**Modern tab:**

> Almighty God, thank You for waking me up and getting me safely to the start of this day. Put Your strong shield around me right now. Don't let me slip into bad habits, mess up, or walk into unnecessary trouble today. Take the steering wheel of my mind so my thoughts aren't spinning out in fear, but are focused on doing what's right and useful, through Jesus Christ our Lord. Amen

### 17. `NEW_prime_starting_this_day` — Starting This Day

- **From:** item(s) 27 of the file; heading was "Starting This Day (Modern English -2026)".
- **Status:** ready
- **Title:** Kept.
- **Why it earns a place:** Plain 42-word rendering; the shortest morning collect in the batch. Not a rumination prayer.
- **Categories:** `morning`, `protection`, `guidance-and-decisions`; **voice:** i; **faiths:** All, Catholic, Anglican, Recovery; **words:** 42
- **see_also:** `tileston_prayers_0408`
- **author:** Traditional — Gelasian Sacramentary
- **attribution:** After the Gelasian collect at Prime (Domine Deus omnipotens). Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Domine Deus omnipotens (Collect at Prime)
- **original_attribution:** Traditional — Gelasian Sacramentary, the collect at Prime. English translation by John, Marquess of Bute, The Roman Breviary (Edinburgh, 1879). Public domain. The same collect is already in the catalog in the Book of Common Prayer wording as tileston_prayers_0408.
- **provenance:** confirmed (in catalog as tileston_prayers_0408)

**Modern tab:**

> God, You brought me to the start of this day -
> protect me today with Your power.
> Keep me from sin and keep me safe from danger.
> Let everything I do and think be led by You to do the right thing.

### 18. `NEW_ambrose_wake_me_up` — Wake Me Up From Sleep

- **From:** item(s) 29 of the file; heading was "Wake Me Up From Sleep (Modern English)".
- **Status:** ready
- **Title:** Kept.
- **Why it earns a place:** Faithful-ish rendering of the hymn's first two stanzas. Praise because it opens by naming who God is.
- **Categories:** `morning`, `praise`, `guidance-and-decisions`; **voice:** i; **faiths:** All, Catholic, Anglican, Protestant, Recovery; **words:** 40
- **author:** St. Ambrose of Milan
- **attribution:** After St. Ambrose of Milan, Splendor paternae gloriae. Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Splendor paternae gloriae (O Splendor of God's Glory Bright)
- **original_attribution:** St. Ambrose of Milan (c. 340–397). English translation credited in the source file to John Henry Newman (1852). PROVENANCE: this English text is the Robert Bridges translation (Yattendon Hymnal, 1899), not Newman's; either way public domain. Correct the translator credit.
- **provenance:** confirmed hymn; translator credit wrong (Bridges 1899, not Newman)

**Modern tab:**

> Brilliant light from God's own light, You bring day into the world.
> Shine on me today. Wake me up from sleep, and help me live pure.
> Guide me now to do what's right, turning my heart completely to You, Lord.

### 19. `NEW_ambrose_walk_in_your_light` — Wake Me Now to Walk in Your Light

- **From:** item(s) 30 of the file; heading was "Wake Me Now To Walk in Your Light (Modern English)".
- **Status:** ready
- **Title:** Kept; title case fixed ("To" -> "to").
- **Why it earns a place:** "Clear my head, so with a clean heart I can get up right now" is the get-up prayer of the batch.
- **Categories:** `morning`, `praise`, `the-circling-mind`, `being-useful`, `guidance-and-decisions`; **voice:** i; **faiths:** All, Catholic, Anglican, Protestant, Recovery; **words:** 57
- **author:** St. Ambrose of Milan
- **attribution:** After St. Ambrose of Milan, Splendor paternae gloriae. Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Splendor paternae gloriae (O Splendor of God's Glory Bright)
- **original_attribution:** St. Ambrose of Milan (c. 340–397). English translation credited in the source file to John Henry Newman (1852). PROVENANCE: this English text is the Robert Bridges translation (Yattendon Hymnal, 1899), not Newman's; either way public domain. Correct the translator credit.
- **provenance:** confirmed hymn; translator credit wrong (Bridges 1899, not Newman)

**Modern tab:**

> Jesus, You're the brightness of God's glory,
> light from light.
> True Sun, break into this day and shine on me.
> Light up my whole body and clear my head,
> so with a clean heart I can get up right now and walk in Your light.
> Guide me to the next right step.
> Through Christ our Lord. Amen

### 20. `NEW_anselm_thoughts_that_go_nowhere` — Deliver Me From Thoughts That Go Nowhere

- **From:** item(s) 32 of the file; heading was "Deliver Me From Thoughts That Go Nowhere (Modern English)".
- **Status:** ready
- **Title:** Kept.
- **Why it earns a place:** "Thoughts that go nowhere" is rumination in six words.
- **Categories:** `the-circling-mind`, `fear-and-worry`, `guidance-and-decisions`, `being-useful`; **voice:** i; **faiths:** All, Catholic, Anglican, Recovery; **words:** 60
- **Text fixes:** Rewrote the tangled line "Guide me now to, and give me energy now for, the next right step." as "Guide me now to the next right step, and give me the energy for it." Original kept in the log.
- **author:** St. Anselm of Canterbury (attributed)
- **attribution:** After a morning prayer credited to St. Anselm of Canterbury (provenance unverified). Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Oratio ad Matutinum (Deliver Me From Idle Thoughts)
- **original_attribution:** Credited in the source file to St. Anselm of Canterbury (1033–1109), English translation 1884. PROVENANCE UNVERIFIED: the Latin could not be matched to Anselm's Orationes. Verify before crediting Anselm; if unverifiable, credit as 'after the manner of Anselm' or as a Kentucky Bootleggers original.
- **provenance:** UNVERIFIED

**Modern tab:**

> My God, my mercy - pull me out of pointless worries.
> Deliver me from thoughts that go nowhere. Point my feet down Your path.
> Let me make real progress today in Your work. Let my life praise Your holy name.
> Guide me now to the next right step, and give me the energy for it.
> Through Christ our Lord. Amen.

### 21. `NEW_anselm_empty_goals` — Rescue Me From Empty Goals

- **From:** item(s) 33 of the file; heading was "Rescue Me From Empty Goals (Modern English)".
- **Status:** ready
- **Title:** Kept.
- **Why it earns a place:** "Lazy, scattered thoughts" and "make real progress today" cover both halves of the shelf's job: stop the loop, get up.
- **Categories:** `the-circling-mind`, `guidance-and-decisions`, `being-useful`, `dedication-and-service`; **voice:** i; **faiths:** All, Catholic, Anglican, Recovery; **words:** 50
- **Text fixes:** Capitalized Your/You've for house style.
- **author:** St. Anselm of Canterbury (attributed)
- **attribution:** After a morning prayer credited to St. Anselm of Canterbury (provenance unverified). Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Oratio ad Matutinum (Deliver Me From Idle Thoughts)
- **original_attribution:** Credited in the source file to St. Anselm of Canterbury (1033–1109), English translation 1884. PROVENANCE UNVERIFIED: the Latin could not be matched to Anselm's Orationes. Verify before crediting Anselm; if unverifiable, credit as 'after the manner of Anselm' or as a Kentucky Bootleggers original.
- **provenance:** UNVERIFIED

**Modern tab:**

> My God and my mercy,
> rescue me from empty goals and lazy, scattered thoughts.
> Point my feet straight down the path of Your commands,
> and give me the energy today to make real progress
> in what You've called me to do, living for Your glory.
> Through Christ our Lord. Amen.

### 22. `NEW_southwell_i_give_you_this_day` — I Give You This Day

- **From:** item(s) 35, 36 of the file; heading was "I Give You This Day (Modern English)".
- **Status:** MERGE PENDING — Paul to approve the merged text
- **Title:** "I Give You This Day" over "Let's Get To Work": the latter reads as a slogan on a shelf; it survives as the closing line.
- **Why it earns a place:** The plainest offering-of-the-day in the batch. Not a rumination prayer.
- **Categories:** `morning`, `gratitude-and-thanksgiving`, `dedication-and-service`, `being-useful`, `guidance-and-decisions`; **voice:** i; **faiths:** All, Catholic, Recovery; **words:** 56
- **Text fixes:** Items 35 ("Let's Get To Work") and 36 ("I Give You This Day") are one prayer twice (59% overlap, identical intent and structure). Proposed merge: 36's line structure, 35's "waste today" idea and closing "Let's get to work." Both source texts are in the log.
- **author:** St. Robert Southwell (attributed)
- **attribution:** After a morning offering credited to St. Robert Southwell, S.J. (provenance unverified). Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Morning Resolution and Offering of the Day
- **original_attribution:** Credited in the source file to St. Robert Southwell, S.J. (1561–1595), modernized 1895. PROVENANCE UNVERIFIED. Verify before crediting Southwell.
- **provenance:** UNVERIFIED

**Modern tab:**

> Jesus Christ, source of everything good,
> thank You for bringing me safely through the night.
> I give You this day, everything I do.
> Guide my hands, guard my mouth, and rule my heart,
> so I don't waste today on anything against Your will.
> Guide me now to the next right step.
> Let's get to work. Amen.

### 23. `NEW_newman_until_my_work_is_done` — Until My Work Is Done

- **From:** item(s) 38 of the file; heading was "Until My Work Is Done (Original English - 19th Century)".
- **Status:** ready
- **Title:** Kept.
- **Why it earns a place:** The plain-spoken Newman. Item 37, the "me" version of the historic text, is NOT added as a record (see the log); the Original tab here is the true historic "us" wording.
- **Categories:** `evening-and-night`, `strength-and-endurance`, `peace`, `presence`, `comfort`; **voice:** i; **faiths:** All, Catholic, Anglican, Recovery; **words:** 58
- **Text fixes:** Item was labelled "Original English - 19th Century" but is a modern rendering ("the busy world goes quiet, the fever of life breaks"); treated as Modern.
- **see_also:** `historic_step12_025`
- **author:** John Henry Newman
- **attribution:** After John Henry Newman (1801–1890), 'O Lord, support us all the day long'. Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Support Us, O Lord, All the Day Long
- **original_attribution:** John Henry Newman (1801–1890), from the sermon 'Wisdom and Innocence' (1843). Public domain. Already in the catalog in 'us' voice as historic_step12_025; the source file's item 37 is that text with pronouns changed to 'me' and is not added as a new record.
- **provenance:** confirmed (in catalog as historic_step12_025)

**Modern tab:**

> Lord, stay with me all day long in this hard life, until the shadows get long and evening comes, the busy world goes quiet, the fever of life breaks, and my work is done.
> Then in Your mercy give me a safe place to stay, holy rest, and peace at the end.
> Through Jesus Christ our Lord. Amen.

### 24. `NEW_newman_stand_by_me` — Stand By Me, Lord

- **From:** item(s) 39 of the file; heading was "Stand By Me, Lord - Evening Rest (Modern English)".
- **Status:** ready
- **Title:** Dropped "- Evening Rest"; the Evening & Night shelf says it.
- **Why it earns a place:** The "hustle and stress" rendering; distinct from item 38 (34% overlap).
- **Categories:** `evening-and-night`, `strength-and-endurance`, `peace`, `presence`; **voice:** i; **faiths:** All, Catholic, Anglican, Recovery; **words:** 45
- **Text fixes:** "grant us safe rest" -> "grant me safe rest" (voice was mixed) "in your care" -> "in Your care"
- **see_also:** `historic_step12_025`
- **author:** John Henry Newman
- **attribution:** After John Henry Newman (1801–1890), 'O Lord, support us all the day long'. Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Support Us, O Lord, All the Day Long
- **original_attribution:** John Henry Newman (1801–1890), from the sermon 'Wisdom and Innocence' (1843). Public domain. Already in the catalog in 'us' voice as historic_step12_025; the source file's item 37 is that text with pronouns changed to 'me' and is not added as a new record.
- **provenance:** confirmed (in catalog as historic_step12_025)

**Modern tab:**

> Stand by me, Lord, through all the hustle and stress of today,
>  right up until evening comes, the noise stops, and my daily work is finished.
>  When night falls, grant me safe rest and deep peace in Your care,
>  through Jesus Christ our Lord. Amen.

### 25. `NEW_stirup_give_me_eagerness` — Give Me Eagerness Today

- **From:** item(s) 41 of the file; heading was "Give Me Eagerness Today (Modern English)".
- **Status:** ready
- **Title:** Kept.
- **Why it earns a place:** The added "eagerness in good works, strength against temptation" petition is the get-up content; the collect itself is already in the catalog. Author corrected from St. Bernard to the Gelasian Sacramentary (see the log).
- **Categories:** `morning`, `being-useful`, `temptation-and-craving`, `wisdom-and-clarity`; **voice:** i; **faiths:** All, Catholic, Anglican, Protestant, Recovery; **words:** 40
- **see_also:** `fellowship_prayers_0118`
- **author:** Traditional — Gelasian Sacramentary
- **attribution:** After the Gelasian Advent collect (Excita, Domine), with an added petition. Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Excita, Domine (Stir Up, We Beseech Thee, O Lord, Our Hearts)
- **original_attribution:** Traditional — Gelasian Sacramentary, the Advent collect; already in the catalog as fellowship_prayers_0118. PROVENANCE: the source file credits this to St. Bernard of Clairvaux with an added sentence ('Grant us this day eagerness in good works, and strength against all temptations') that has no known source. The Original tab carries only the collect; the added sentence lives in the Modern tab under the Kentucky Bootleggers credit.
- **provenance:** confirmed collect (fellowship_prayers_0118); Bernard credit wrong; added sentence unsourced

**Modern tab:**

> Lord, fire up my heart to get ready for Jesus to come;
> make my mind clear so I can serve You right.
> Give me eagerness today to do good work, and
> strength and energy to push through every temptation.
> Amen

### 26. `NEW_stirup_make_today_count` — Let's Make Today Count

- **From:** item(s) 42 of the file; heading was "Let's Make Today Count (Modern English)".
- **Status:** ready
- **Title:** Kept.
- **Why it earns a place:** Informal register of the same; "brave enough" adds Courage.
- **Categories:** `morning`, `being-useful`, `temptation-and-craving`, `courage`, `wisdom-and-clarity`; **voice:** i; **faiths:** All, Catholic, Anglican, Protestant, Recovery; **words:** 47
- **see_also:** `fellowship_prayers_0118`
- **author:** Traditional — Gelasian Sacramentary
- **attribution:** After the Gelasian Advent collect (Excita, Domine), with an added petition. Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Excita, Domine (Stir Up, We Beseech Thee, O Lord, Our Hearts)
- **original_attribution:** Traditional — Gelasian Sacramentary, the Advent collect; already in the catalog as fellowship_prayers_0118. PROVENANCE: the source file credits this to St. Bernard of Clairvaux with an added sentence ('Grant us this day eagerness in good works, and strength against all temptations') that has no known source. The Original tab carries only the collect; the added sentence lives in the Modern tab under the Kentucky Bootleggers credit.
- **provenance:** confirmed collect (fellowship_prayers_0118); Bernard credit wrong; added sentence unsourced

**Modern tab:**

> Stir up my heart, Lord, to get ready for what You want me to do today.
> Purify my mind, give me genuine excitement and energy for good work.
> Make me strong enough and brave enough to beat every temptation.
> Let's make today count. In Jesus name, Amen.

### 27. `NEW_coptic_light_up_my_heart` — Light Up My Heart

- **From:** item(s) 45 of the file; heading was "Light Up My Heart (Modern English)".
- **Status:** ready
- **Title:** Kept.
- **Why it earns a place:** 30 words; the shortest morning prayer in the batch. Item 44 (the same 1911 translation with pronouns modernized, same title) is dropped as having no content of its own.
- **Categories:** `morning`, `guidance-and-decisions`, `wisdom-and-clarity`; **voice:** i; **faiths:** All, Oriental Orthodox, Recovery; **words:** 30
- **Text fixes:** "Jesus, our God, You showed me" -> "Jesus, my God, You showed me" (voice was mixed)
- **author:** Traditional — Coptic Agpeya (attributed)
- **attribution:** After a morning prayer credited to the Coptic Agpeya, Third Hour (provenance unverified). Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Morning Prayer of the Third Hour (Agpeya)
- **original_attribution:** Credited in the source file to the Coptic Agpeya, Third Hour, English translation 1911 (Marcus Simaika Pasha). PROVENANCE UNVERIFIED: the text could not be matched to the Agpeya and Simaika is not a known translator of it. Verify before crediting the Coptic tradition.
- **provenance:** UNVERIFIED

**Modern tab:**

> Jesus, my God, You showed me Your true light.
> Light up my heart and soul with Your light.
> Teach me to walk straight on Your path all day long.
> Amen.

### 28. `NEW_coptic_one_true_light` — The One True Light

- **From:** item(s) 46 of the file; heading was "The One True Light (Modern English)".
- **Status:** ready
- **Title:** Kept.
- **Why it earns a place:** "Set my heart and mind on fire with Your presence this morning" is Presence and Morning.
- **Categories:** `morning`, `guidance-and-decisions`, `presence`; **voice:** i; **faiths:** All, Oriental Orthodox, Recovery; **words:** 38
- **Text fixes:** "walk straight the path" -> "walk straight down the path" (missing word)
- **author:** Traditional — Coptic Agpeya (attributed)
- **attribution:** After a morning prayer credited to the Coptic Agpeya, Third Hour (provenance unverified). Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Morning Prayer of the Third Hour (Agpeya)
- **original_attribution:** Credited in the source file to the Coptic Agpeya, Third Hour, English translation 1911 (Marcus Simaika Pasha). PROVENANCE UNVERIFIED: the text could not be matched to the Agpeya and Simaika is not a known translator of it. Verify before crediting the Coptic tradition.
- **provenance:** UNVERIFIED

**Modern tab:**

> Lord Jesus Christ, You are the one true light.
> Set my heart and mind on fire with Your presence this morning.
> Guide my steps so I walk straight down the path of
> Your commands today without hesitation.
> Amen.

### 29. `NEW_armenian_morning_till_night` — From Morning Till Night

- **From:** item(s) 49 of the file; heading was "From Morning til Night (Modern English)".
- **Status:** ready
- **Title:** Spelling fixed; otherwise kept.
- **Why it earns a place:** Item 48 (the 1901 translation with pronouns modernized plus "In the name of Jesus, I pray") is dropped as having no content of its own beyond the Original tab; this is the one genuinely modern rendering.
- **Categories:** `morning`, `being-useful`, `dedication-and-service`, `trust-and-faith`; **voice:** i; **faiths:** All, Armenian Apostolic, Oriental Orthodox, Recovery; **words:** 42
- **Text fixes:** Title: "til" -> "Till"
- **author:** St. Nerses Shnorhali (attributed)
- **attribution:** After a morning prayer credited to St. Nerses Shnorhali (provenance doubtful). Adapted by Kentucky Bootleggers, 2026.
- **original_title:** Morning Prayer of Faith and Action (after Nerses Shnorhali)
- **original_attribution:** Credited in the source file to St. Nerses IV Shnorhali (1102–1173), English translation by J. Issaverdens (1901). PROVENANCE DOUBTFUL: the text does not match the known verses of Shnorhali's 'I Confess with Faith'. Verify before crediting Nerses.
- **provenance:** DOUBTFUL

**Modern tab:**

> Christ, I trust in You and bow before You.
> You are maker of the morning light and the whole world.
> Point my feet directly toward good work today.
> Give me the strength to live out Your will from morning till night.
> Amen.

