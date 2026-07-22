# PLE — Portuguese course roadmap

> Status: scaffold → build. This roadmap turns the empty `ple` repo (LICENSE + README + brand icons, "coming soon" on the world map) into a complete boulingua Portuguese course — content **and** website — conforming to the `pagegen` template, the `curriculum` framework, and the VG Wort standard. Signature accent `#B723C7` / dark `#DC7EE7`; site `code = ple` (already present in `data/accents.yaml`).

---

## 1. Overview

**What this course is.** *Português Língua Estrangeira (PLE)* — a free, openly-licensed Portuguese course for the German *Gesamtschule* system and for independent adult learners, built on the boulingua five-step unit model (**Activate → Input → Practise → Apply → Reflect**), with committed slide decks, printable worksheets, native-voice audio, and CEFR-traceable can-dos.

**Realistic CEFR scope.** Target **A1 → B1** as the shipped course (framework conformance level `core`), fronted by a **Pre-A1 "Sons & Sinais" onboarding stage** (pronunciation + orthography), with **B2** declared as a stretch phase, not a launch requirement. This matches what a solo OER author can author and maintain to a high standard, and mirrors the sister sites (DaF/EFL/FLE).

**Why Portuguese.** Sixth-most-spoken language worldwide, official in nine countries across four continents, high German-learner and migration relevance, and a Romance language that transfers well from the existing FLE/ELE/ILS learner base — yet under-served by free, CEFR-aligned OER.

**The 3–5 defining challenges (from the language-specific context):**
1. **Variant fork (pt-PT vs pt-BR).** The single biggest up-front decision — it cascades into orthography, vocabulary, morphology (e.g. *tu* vs *você*, gerund vs infinitive constructions), and the TTS voice. Must be decided before unit 1.
2. **Orthography under the 1990 Acordo Ortográfico.** Post-reform spelling reduced but did not eliminate PT/BR divergence; the course must pick one norm and apply it consistently across prose, materials, and audio scripts.
3. **Phonology is hard for German L1 learners.** Nasal vowels (*ão, ãe, õe*), the digraphs *lh/nh*, *ç*, open/closed vowel contrasts, and — in pt-PT — heavy vowel reduction. Justifies a dedicated Pre-A1 sound stage.
4. **Latin script, but diacritics-dense.** No script-onboarding barrier, but fonts and audio pipelines must handle *á â ã à ç é ê í ó ô õ ú* cleanly.
5. **Register/pronoun politeness system** differs by variant and is pedagogically load-bearing from A1 (address forms, *o senhor/a senhora*, clitic placement).

---

## 2. Language & localisation decisions

Each item states a concrete, opinionated recommendation.

- **Writing system / script handling.** Latin script; **no level-0 alphabet stage needed**. But diacritics and digraphs are pervasive, so **do** ship a Pre-A1 onboarding stage focused on *sounds and spelling*, not letters (see §5).
- **Variant / norm — the pivotal choice.** **Recommendation: European Portuguese (pt-PT) as the primary teaching norm**, `languageCode = "pt-PT"`, post-1990 orthography. Rationale: consistency with the European school context the sister sites serve, and learner destination realism for German-based learners heading to Portugal. **Every unit carries a compact "No Brasil" contrast box** (pronoun, vocabulary, and pronunciation deltas) so the course stays usable for Brazilian-oriented learners without forking the repo. This is the one decision that is expensive to reverse — lock it in `hugo.toml` and the style guide before authoring unit 1.
- **RTL:** not applicable (LTR).
- **Web fonts.** Keep hugo-coder's default stack; **no custom web font required** — the needed Latin+diacritics coverage is universal. Only verify the materials pipeline (LaTeX) fonts render *ã/õ/ç* (they do under the standard slidegen/sheetgen templates). Do not add a font just for Portuguese.
- **Native-voice / Piper TTS.** **Available for both variants.** Recommendation: **pt-PT `tugão` (medium)** as the course voice to match the norm decision; keep a **pt-BR voice (e.g. `faber`/`edresson`) on hand** for the contrast boxes and for a possible future Brazilian track. Wire through the shared `audiogen`/Piper workflow; commit generated audio under `static/`.
- **Level-0 stage.** Yes — **Pre-A1 "Sons & Sinais"**: 3–4 short editorial pages (vowel/nasal system, *lh/nh/ç*, stress & accent marks, PT vs BR spelling at a glance). Editorial, VG-Wort-eligible where ≥1800 chars.

---

## 3. Instantiation from pagegen

Stand up the buildable site first; author content second.

1. **Copy the template** into the repo working tree: bring `pagegen/`'s `archetypes/ assets/ brand/ content/ data/ i18n/ layouts/ scripts/ static/ _materials/ hugo.toml go.mod go.sum .github/ .gitignore .gitattributes lychee.toml LICENSE* CITATION.cff` into `ple/` (preserving the existing `ple/brand/` accent assets and README/LICENSE).
2. **Edit only the marked `hugo.toml` values:**
   - `baseURL = "https://boulingua.github.io/ple/"`
   - `title = "Português (PLE) — S. Le Boulanger"`
   - `languageCode = "pt-PT"`, `defaultContentLanguage = "pt-PT"` (BR would be `pt-BR`)
   - `[params].navTitle = "Português"`, `description`, `keywords` (Portuguese, PLE, CEFR, OER, Gesamtschule…)
   - `[[params.social]].url = "https://github.com/boulingua/ple"`
   - `[params.plausible].domain = "boulingua.github.io/ple"`
   - `[params].code = "ple"` (drives the accent — **do not touch CSS**)
   - `[[menu.main]]` sections mirroring the real `content/ple/` tree (Pre-A1, A1, A2, B1; Materials; About; Legal). Keep the `[params.plausible]` sub-table **last** (the TOML sub-table trap noted in the template).
3. **Regenerate the pentagon + favicons:** `python brand/make_icon.py` (reads `code=ple` → `#B723C7`), confirm `brand/icon.svg/png` and static favicons update.
4. **Confirm `data/accents.yaml` carries `ple`** — it already does (accent `#B723C7`, hover `#931CA0`, dark `#DC7EE7`, dark_hover `#E8A9EF`). No edit needed.
5. **Fill the ⟨…⟩ placeholders** in `impressum.md`, `datenschutz.md` (include the VG Wort METIS disclosure), `haftungsausschluss.md`.
6. **First green build + Pages:** `hugo` builds clean; enable the `build-deploy.yml` workflow; confirm GitHub Pages serves the placeholder site with correct accent and icon. This is the MVP-0 milestone — a live, correctly-branded, empty course.

---

## 4. Curriculum conformance target

- **Declared level: `core` (A1–B1).** Requirement: implement **every in-scope scale that has an official CEFR descriptor at A1, A2, or B1**; record `no-official-descriptor` for empty cells (never silently skip). `full`/`complete` (B2–C2) are explicitly declared **unmet** at launch — the honest-gap posture the framework endorses.
- **Scales implemented vs declared-empty.** Prioritise, at A1–B1, the reception/production/interaction scales that carry descriptors at those levels (REC oral/reading comprehension, PROD oral/written production, INT conversation/information-exchange/transactions/correspondence, LING range/accuracy/vocabulary/phonology/orthography, SOC appropriateness, PRAG fluency/coherence). **Declare `no-official-descriptor`** honestly for the many MED (mediation strategy) and PLUR scales that lack A1 cells, and cover the mediation/plurilingual scales that *do* have B1 descriptors as the course reaches B1.
- **Mapping units → descriptor IDs.** Each unit's front-matter `curriculum` block (framework `cefr`) lists `cefr_can_do` realizations; each realization maps to a framework ID `{LEVEL}.{DOMAIN}.{SCALE}.{SEQ}` (e.g. `A1.INT.information-exchange.01`, `A2.REC.overall-oral-comprehension.02`). The Portuguese realization is the *pt* wording; the framework holds the grounded CEFR descriptor — neither reproduces Council of Europe text.
- **Machine-readable scope/coverage manifest.** Publish a `conformance.yml` at repo root modelled on `curriculum/examples/de-a1/conformance.yml`: `framework_version`, `language: pt`, `declared_conformance: core`, and `realizations[]` each with `implements_id` + `pt` text. Every `implements_id` must resolve against `curriculum/levels/*.md`. Gate it in CI by running **`curriculum/scripts/id-audit.sh`** (format + global uniqueness + scale/level resolution) against the course's IDs; the build fails on any unresolved or malformed ID.

---

## 5. Content creation plan

Recurring **cast & theme**: a small ensemble (e.g. *Rita* in Lisboa, *Tomás* the student, *Dona Amélia* the neighbour, plus an Erasmus friend) recurring across units so vocabulary and dialogues compound. Exams are **first-class sibling bundles** (`…-exam/`), sharing `unit_nr` with their unit. Effort tags: **S** ≈ ½ day, **M** ≈ 1–2 days, **L** ≈ 3–5 days per unit incl. materials + audio.

**Phase P0 — Pre-A1 "Sons & Sinais" onboarding** (S–M). 3–4 editorial pages: vowels & nasals, *lh/nh/ç*, stress & accent marks, PT-vs-BR spelling snapshot. Acceptance: each ≥1800 chars where eligible, audio for every sound, one worksheet.

**Phase P1 — A1 (MVP course)** — **~12 units + 12 exams** (each **M**). Topics: greetings & introductions, family, home/room, school/work day, food & drink, shopping & prices, city & directions, daily routine, time & weather, free time, past-weekend basics, review. Acceptance per unit: 5-step body; `skills_focus` set; ≥6 CEFR realizations mapped to A1 IDs; committed deck + worksheet + answer key + audio; exam sibling with `total_points`/`notenschluessel`; VG Wort mark registered.

**Phase P2 — A2** — **~12 units + 12 exams** (each **M–L**). Topics: past narration (pretérito perfeito/imperfeito), travel & transport, health & body, housing & services, work & CVs, plans & future, comparisons, recipes & instructions, media & opinions, etc.

**Phase P3 — B1** — **~10–12 units + exams** (each **L**). Topics: opinion & argument, formal correspondence, subjunctive introduction, workplace interaction, culture & society across the Lusophone world, mediation tasks (relaying/summarising), online discussion.

**Phase P4 — Appendices** (S–M each): Portuguese–German glossary, verb-conjugation reference (regular + high-frequency irregulars), common German-L1 errors, pronunciation guide (expanded P0), writing rubrics, skills decision tree, teaching workflow, PT/BR contrast appendix.

**Phase P5 — B2 (stretch, post-launch)** — declared, not required for going active.

Per-phase acceptance criteria: clean `hugo` build; all units have deck+worksheet+audio committed; every editorial page ≥1800 chars has a VG Wort mark; `id-audit.sh` green; link-check green.

---

## 6. Website & materials

- **Section landings via shortcodes** (never raw HTML): `_index.md` per section using `{{< hero >}}`/`{{< kicker >}}`/`{{< lead >}}` as in the archetype. Sections: `/pre-a1/`, `/level-a1/`, `/level-a2/`, `/level-b1/`, `/materials/`, `/about/`.
- **Materials pipeline.** Generate decks (**slidegen**, editable `.odp` + PDF) and worksheets (**sheetgen**, printable PDF) locally from the branded LaTeX templates; **commit** outputs under `static/materials/{presentations,worksheets}` and `static/downloads/<level>/`. CI **verifies only** (no TeX Live in the deploy path — `verify_downloads.py`, `pdf_attribution.py`).
- **Native-voice audio.** Use **audiogen/Piper**; **pt-PT `tugão` (medium)** primary voice, pt-BR voice for contrast clips. Availability for Portuguese is **confirmed for both variants** — no blocker. Build per `AUDIO_STRUCTURE.md`; commit audio; reference in unit front matter.
- **Thumbnails** via `render_thumbs.py` for every deck/worksheet (`presentation.thumbnail`, `worksheet.thumbnail`).
- **Downloads** surfaced on unit pages and the materials hub; the hub is navigation → **no VG Wort pixel** (hub guard enforces `met.vgwort.de` absence).

---

## 7. VG Wort — pixel assignment for ALL content pages (required)

**Non-skippable.** Every original creative page ≥1800 rendered characters gets exactly one VG Wort Zählmarke, per `pagegen/docs/vgwort-standard.md`.

- **Which pages get a mark:** every **unit**, every **exam**, every **appendix**, and every editorial page (Pre-A1 sound pages, About/course-overview prose) that reaches 1800 chars. **No mark** on: home, `/materials/` hub, tag/level/topic indexes, `/page/N/` continuations, or the three templated legal pages.
- **Codes.** Draw **fresh public codes** (32-hex "Öffentlicher Identifikationscode") from the author's **T.O.M.** account — never invent, never expose the private code. Served from `https://vg09.met.vgwort.de/na/<code>`.
- **Registration.** Add each to `data/vgwort.yaml` keyed by `url:` (or `path:`) with `public_id`/`pixel_url`, `min_chars: 1800`, `author`, `registered_at`. Rendering goes solely through `layouts/_partials/vgwort/url.html` → `<head>` preload + eager `<body>` `<img>` (no JS, no consent gate, `loading="eager"`, off-screen not `display:none`).
- **Registry.** Record every code in the **private** usage registry (`Used`, `Projekt=ple`, `Sprache=pt`, `Niveau`, `Kurstitel`, `URL`, `Pixel_URL`) kept outside the repo.
- **Gates.** Coverage audit (`verify_vgwort_coverage.py`, warns on unregistered ≥1800-char pages), render verify (`verify_rendered_pixels.py`, blocking — each registered pixel present on exactly one URL), hub guard (blocking).
- **Estimated total marks needed:** Pre-A1 (~3) + A1 (12 units + 12 exams) + A2 (12 + 12) + B1 (~11 + 11) + appendices (~7) + editorial (~3) ≈ **~86 marks for the core A1–B1 launch**; add ~22 more (~**108 total**) if B2 ships later. Draw codes in batches per phase, not all at once.

---

## 8. Milestones & sequencing

1. **M0 — Live scaffold** (§3). Template instantiated, `code=ple` accent live, icon regenerated, legal placeholders filled, first green Pages deploy. *No content yet.* Dependency: none.
2. **M1 — Framework wiring.** `conformance.yml` (`declared_conformance: core`), `id-audit.sh` in CI, style guide locking **pt-PT + 1990 orthography**, cast bible, VG Wort T.O.M. batch #1 drawn. Dependency: M0.
3. **M2 — Pre-A1 + A1 MVP (public flip candidate).** P0 onboarding + all A1 units/exams/materials/audio, marks registered, gates green. **This is the minimum to flip `ple` from "coming soon" to active on the world map.** Dependency: M1.
4. **M3 — A2 complete.** Dependency: M2.
5. **M4 — B1 complete → `core` conformance fully met.** Dependency: M3.
6. **M5 — Appendices + polish** (glossary, verb tables, rubrics, PT/BR contrast). Dependency: M4.
7. **M6 (stretch) — B2** and any Brazilian-track spin-off. Dependency: M5.

**Definition of done / ready to go active:** M2 shipped — Pre-A1 + a coherent A1 level live, every content page carrying a registered VG Wort mark, all decks/worksheets/audio committed, `id-audit.sh` + VG Wort render/coverage + downloads + legal + link-check gates green, menus mirroring the real tree, and the About page explaining the pt-PT decision. Full `core` completion is M4.

---

## 9. Open decisions & risks (language-specific)

- **Variant lock-in (highest impact).** pt-PT is the recommendation; reversing after authoring is costly. *Mitigation:* decide at M1, encode in the style guide, and keep all BR divergence isolated in the "No Brasil" boxes so a future Brazilian track can branch cleanly rather than rewrite.
- **Orthography drift.** Post-1990 spelling still has PT/BR variants (e.g. *facto/fato*, accentuation). *Mitigation:* a fixed word-list in the style guide; a lint check for banned pre-reform spellings.
- **TTS naturalness.** pt-PT Piper (`tugão`) is serviceable but heavier vowel reduction can sound off on isolated words. *Mitigation:* audit generated clips; prefer full phrases; hand-flag any mispronounced items.
- **Pronoun/register pedagogy.** *tu* vs *você* vs *o senhor* is genuinely divergent by variant and region. *Mitigation:* teach the pt-PT system as default, surface BR usage in contrast boxes, keep it explicit from A1.
- **Descriptor honesty.** Many MED/PLUR scales lack A1 descriptors; the temptation is to invent coverage. *Mitigation:* record `no-official-descriptor` and let `id-audit.sh` enforce that only resolvable IDs ship.
- **Solo-author throughput.** ~86 VG-Wort-eligible pages plus materials and audio is a large surface. *Mitigation:* ship by phase (M2 flips the map early), reuse the recurring cast to compound content, and batch materials/audio generation per level.
