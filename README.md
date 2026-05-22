# Build Vocabulary CSV

AI skill for creating, adapting, migrating, and validating vocabulary CSV files for language-learning projects.

## What It Does

- Builds new vocabulary CSVs from a topic.
- Mirrors an existing CSV's schema, concept order, and style.
- Migrates legacy CSVs into richer multilingual schemas.
- Adds concise definitions, annotations, parts of speech, and locale-specific fields.
- Validates CSV structure with real CSV parsers instead of manual line splitting.

## Default Schema

When no schema is provided, the skill uses a simple multilingual format:

```csv
word,definition.en,definition.ja,definition.zh-TW,pos,annotation.en,annotation.ja,annotation.zh-TW,photoURL
```

The schema can be adjusted for the requested languages or matched exactly to an existing source CSV.

## Use Cases

- Generate a beginner vocabulary deck for a topic.
- Translate or adapt an existing deck into another language.
- Preserve row order while adding new locale columns.
- Check that generated CSVs are parseable and complete.

## Files

- `SKILL.md` - Skill instructions and workflow.
- `agents/openai.yaml` - OpenAI agent metadata for the skill.
- `LICENSE` - MIT license.

## License

MIT
