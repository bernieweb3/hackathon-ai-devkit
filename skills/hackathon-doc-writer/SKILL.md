# hackathon-doc-writer

## Goal
Generate structured technical documentation artifacts (ADR, PRD, feature specs) for a hackathon project using the appropriate template.

---

## Inputs

| Input | Type | Required | Description |
|---|---|---|---|
| `document_type` | enum | Yes | One of: `ADR`, `PRD`, `feature-spec` |
| `project_title` | string | Yes | Name of the project |
| `problem_statement` | string | Yes | Core problem being solved |
| `mvp_features` | string[] | Yes | MVP feature list from `hackathon-scope-cutter` |
| `tech_stack` | string[] | Yes | Technologies being used |
| `architecture_decisions` | object[] | No | For ADR: list of decisions with context and rationale |
| `feature_name` | string | No | For feature-spec: name of the specific feature |
| `constraints` | string[] | No | Technical or product constraints |

---

## Outputs

| Output | Description |
|---|---|
| `document` | Fully populated document in Markdown |
| `document_type` | Type of document generated |
| `missing_fields` | Sections that could not be completed due to missing input |

---

## Rules

1. Use the corresponding template from `templates/` directory as the output structure.
2. Do not omit any section from the template; use `[TBD]` for missing information.
3. Keep language direct and scannable — no filler paragraphs.
4. For ADR: capture exactly one architectural decision per document.
5. For PRD: include success metrics even if they are estimates.
6. For feature-spec: include acceptance criteria as testable conditions.
7. Flag all `[TBD]` sections in `missing_fields` output.

---

## Output Format

```yaml
document_type: "<ADR|PRD|feature-spec>"

missing_fields:
  - "<section name>"

document: |
  <Full Markdown document content>
```
