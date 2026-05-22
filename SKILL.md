---
name: build-vocabulary-csv
description: Build, adapt, translate, migrate, or validate vocabulary CSV files for language-learning projects without depending on a specific app or repository. Use when Codex is asked to generate a new vocabulary CSV from a topic, mirror an existing CSV pattern in another language, add multilingual definition or annotation columns, preserve concept order across counterpart CSVs, or validate language-learning CSV files.
---

# Build Vocabulary CSV

## Core Workflow

1. Identify the requested CSV mode.
   - **Standalone mode**: Build a new vocabulary list from a topic, such as plant names, airport vocabulary, verbs, occupations, or weather.
   - **Mirror mode**: Use one or more existing CSVs as the concept/order/source pattern.
   - **Migration mode**: Convert a legacy CSV into a richer schema while preserving rows and existing fields.

2. Determine the schema.
   - If the user provides a header, use it exactly.
   - If an existing CSV is being mirrored, match its header unless the user asks for a new schema.
   - If no schema is provided, propose or use a simple multilingual schema:

   ```csv
   word,definition.en,definition.ja,definition.zh-TW,pos,annotation.en,annotation.ja,annotation.zh-TW,photoURL
   ```

   Adjust locales to the user’s requested languages. Keep `photoURL` empty unless URLs are provided.

3. Build the vocabulary list.
   - Use the target language term in `word`.
   - Use common learner-facing terms before obscure or technical terms.
   - If no row count is provided, choose a conservative category size, usually 30-50 rows.
   - Keep one row per concept.
   - Use stable ordering: broad/common terms first, then related subgroups, then less common but useful terms.
   - Avoid scientific, regional, slang, or overly specialized terms unless requested.

4. Write short definitions and useful annotations.
   - Definitions should be concise glosses.
   - Annotations should explain usage, register, ambiguity, grammar, irregularity, or memorable examples.
   - Use the requested writing system and locale. For Traditional Chinese, use Traditional characters.
   - For verbs/adjectives, use canonical dictionary forms unless the user requests conjugated forms.

5. Validate the CSV.
   - Use a structured CSV parser, not manual line splitting.
   - Confirm every row has the same number of fields as the header.
   - Confirm required fields are non-empty.
   - Confirm row count matches the user’s request or the mirrored source.
   - Confirm quoted commas and quotes parse correctly.

## `pos` Field Guidance

If the schema includes a `pos` column, use it for part of speech or expression type. Use the project’s accepted values if one exists. If not, prefer simple lowercase labels:

- `noun`
- `verb`
- `adjective`
- `adverb`
- `pronoun`
- `preposition`
- `conjunction`
- `interjection`
- `particle`
- `phrase`
- `unknown`

If the schema does not include `pos`, do not add it unless the user asks for it. If mirroring an existing schema that uses another name, such as `form` or `partOfSpeech`, preserve that existing column name. Do not invent specialized labels unless the user’s project already uses them.

## Standalone CSV Guidance

For a new topic without counterpart files:

- Ask only if the missing decision materially changes the output, such as target language, target locales, or row count.
- Otherwise choose sensible defaults and proceed.
- For plant CSVs, prefer common plant names; avoid Latin scientific names unless requested.
- For travel CSVs, prioritize phrases and nouns a beginner would encounter.
- For job CSVs, prefer common occupations and standard terms.
- For grammar CSVs, include particles, auxiliary verbs, or set phrases only if the schema supports them.

## Migration Guidance

When converting a legacy CSV:

- Preserve the original row order.
- Preserve existing definitions and annotations.
- Add new locale columns beside existing locale columns.
- Do not discard columns unless the user explicitly asks.
- Keep the final CSV parseable by standard CSV tooling.

## Validation Examples

Use any available CSV parser. Examples:

```bash
ruby -rcsv -e 'rows=CSV.read("vocabulary.csv", headers:true); puts rows.length; puts rows.headers.join("|")'
```

```bash
node --input-type=module -e "import { parse } from 'csv-parse/sync'; import { readFileSync } from 'fs'; const rows = parse(readFileSync('vocabulary.csv'), { columns: true, skip_empty_lines: true }); console.log(rows.length);"
```

## Output Principles

- Prefer editing or creating the actual CSV file when working in a shared workspace.
- Mention assumptions briefly in the final response.
- Report validation commands and results.
- If the CSV is generated from scratch, mention that there was no counterpart source and that the list is newly curated.
