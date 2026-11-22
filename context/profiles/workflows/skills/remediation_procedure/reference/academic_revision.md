# Academic Revision

Execute systematic multi-pass revision of academic documents

Revision proceeds through four sequential passes, each targeting a specific quality dimension. Mixing concerns across passes reduces detection accuracy because structural problems obscure sentence-level issues, and premature word-level edits get discarded during restructuring.

## Revision Triggers

Initiate formal revision when any condition applies:

- **Draft completion**: All sections written, argument structure finalized
- **Structural change**: Sections added, removed, or reordered
- **Feedback received**: Peer review, supervisor comments, or validation findings require integration
- **Time gap**: More than one week since last substantive edit (fresh perspective available)

Minor edits (typo fixes, citation updates) do not require a full four-pass cycle. Reserve the full procedure for substantive revision events.

## Pass 1: Structure and Logic

1. Read the document continuously without editing
2. Outline the actual argument flow as it exists on the page
3. Compare the actual flow against stated research questions
4. Identify logical gaps, unsupported transitions, orphaned paragraphs, and circular arguments
5. Check that each paragraph advances exactly one main idea
6. Verify the argument thread connects introduction through conclusion
7. Restructure to resolve identified issues before proceeding

Do not edit sentences or words during this pass. Structural changes invalidate sentence-level work.

Output: a revised document outline reflecting the actual argument structure, with annotations marking any sections that moved, merged, or split.

## Pass 2: Sentence Variety

1. Measure sentence length distribution across a representative section (minimum 20 sentences)
2. Target distribution: 70% medium (15-25 words), 20% short (8-14), 10% long (26-35)
3. Check sentence openings: vary subject-first, transitional, adverbial, and prepositional starts
4. Check sentence types: mix simple, compound, and complex structures
5. Verify no transition word repeats within any single section
6. Break sequences of three or more sentences sharing the same syntactic pattern
7. Read the section aloud to catch repeated openers, runaway sentences, and mechanically even beats

Uniform sentence rhythm signals mechanical writing. Even well-argued content loses readers when every sentence follows the same cadence.

## Pass 3: Word Choice and Precision

1. Audit technical terms: each used consistently throughout, defined on first use
2. Check hedging frequency: approximately one hedge per 50 words for uncertain claims, zero for established facts
3. Replace vague quantifiers with specific values ("several" with exact count, "significant" with statistical significance or "meaningful")
4. Remove jargon that adds no precision beyond simpler alternatives
5. Verify active voice dominates (target 80% active, 20% passive)

## Pass 4: Engagement and Neutrality

1. Search for negation-for-drama patterns ("not just X, but Y"; "it's not X, it's Y") and rewrite as affirmative statements
2. Search for formulaic expressions (leverage, streamline, delve, underscore, landscape, tapestry) and replace with direct alternatives
3. Search for filler phrases ("In the ever-evolving...", "It is crucial to recognize...") and delete or rewrite
4. Verify each claim is proportional to its supporting evidence
5. Check tone: measured, specific, authoritative without overstatement

This pass targets patterns that erode reader trust. A single formulaic phrase can undermine an otherwise rigorous section.

## Pass Sequencing

The four passes follow a deliberate order. Structure (Pass 1) must stabilize before measuring sentence variety (Pass 2), because restructuring adds, removes, and relocates sentences. Word choice (Pass 3) depends on stable sentences. Engagement and neutrality (Pass 4) operates on finalized prose where pattern searches yield accurate results.

Reversing or interleaving passes creates rework. A word-level edit in a paragraph that later moves during restructuring wastes effort and may introduce inconsistency.

## Post-Revision Verification

After completing all four passes:

- [ ] Argument flows logically from introduction through conclusion
- [ ] Sentence length distribution within target ranges
- [ ] No anti-pattern vocabulary detected
- [ ] All claims backed by cited evidence
- [ ] Technical terms consistent and defined on first use
- [ ] Transitions varied and natural across sections
- [ ] Hedging calibrated to claim certainty level
- [ ] No three consecutive sentences share the same syntactic pattern
- [ ] Active voice used in at least 80% of sentences

## Anti-Patterns

| Anti-Pattern | Consequence | Alternative |
|--------------|-------------|-------------|
| Editing during Pass 1 read-through | Disrupts structural assessment; edits get discarded during restructuring | Read first, restructure second |
| Combining passes into a single read | Attention splits across dimensions, reducing detection rate | Complete each pass fully before starting the next |
| Skipping Pass 2 metrics | Sentence uniformity persists undetected | Measure at least one representative section quantitatively |
| Revising without a time gap | Familiarity blindness hides errors | Wait at least 24 hours between drafting and Pass 1 |

`ctx:github.com/ctxmod/writing_academic/context::shared::process@scholarly_prose/reference/revision.md`
`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/anti_patterns.md`
`ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/academic_language.md`
`ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/terminology.md`
`ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/academic_register.md`
`ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/rhythm.md`
`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/quality_assurance.md`
