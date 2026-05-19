# Power BI Developer — Project Guidelines

## Project Overview

This is an AI-driven Power BI development workspace for the **Adventure Works DW 2020** solution.
It uses the **PBIP** (Power BI Project) format with TMDL-based semantic model definitions and Git-friendly report JSON files.

- Report: `Adventure Works DW 2020.Report/`
- Semantic Model: `Adventure Works DW 2020.SemanticModel/`
- DAX Queries: `Adventure Works DW 2020.SemanticModel/DAXQueries/`

## Semantic Model — MCP Only

**Never read `.tmdl` files directly** from `*.SemanticModel/definition/`.
Always query the live model through the `powerbi-modeling-mcp` MCP server tools to get accurate, up-to-date state:

- Tables, columns, measures → `powerbi-model_table_operations`, `powerbi-model_column_operations`, `powerbi-model_measure_operations`
- Relationships → `powerbi-model_relationship_operations`
- DAX expressions → `powerbi-model_dax_query_operations`
- Calculation groups → `powerbi-model_calculation_group_operations`

## DAX Conventions

- Use **CALCULATE** with explicit filter arguments — never rely on implicit filter context assumptions
- Prefer **DIVIDE(numerator, denominator, 0)** over `/` to avoid division-by-zero errors
- Use **VAR / RETURN** blocks for any expression longer than one line
- Measure names use **Title Case** with spaces (e.g., `Total Sales Amount`, `YTD Revenue`)
- Time intelligence measures must reference the `Date` table (marked as date table)
- Format strings: currency as `"#,0.00 €"`, percentages as `"0.00%"`

## Data Modeling Conventions

- Fact tables: `Sales`, `Currency Rate`
- Dimension tables: `Customer`, `Date`, `Product`, `Reseller`, `Sales Territory`, `Currency`, `Sales Reason`, `Sales Order`
- Bridge tables: `Sales Reason Bridge`
- All relationships follow a **star schema** — avoid chained or complex many-to-many unless necessary
- Use **single-direction** cross-filter by default; enable bidirectional only with explicit justification
- Hidden columns used only as sort-by or key columns must have `isHidden: true`

## Power Query / M Conventions

- Named expressions and parameters live in `expressions.tmdl`
- Use **query folding** wherever possible — validate with the query diagnostics tool
- Parameter names use **PascalCase** (e.g., `ServerName`, `DatabaseName`)

## Workspace Structure

```
.github/
  copilot-instructions.md     ← This file (project-wide, always on)
  agents/                     ← Custom agent personas
  skills/                     ← Multi-step workflow skills
Adventure Works DW 2020.Report/
Adventure Works DW 2020.SemanticModel/
```

## Available Copilot Customization Artifacts

| Artifact | Location | Purpose |
|----------|----------|---------|
| Prompt-Engineer agent | `.github/agents/Prompt-Engineer.agent.md` | Create/review Copilot customization files |
| Modell-Auditor agent | `.github/agents/Modell-Auditor.agent.md` | Read-only audit of the semantic model against project conventions |
| Prompt-Engineering skill | `.github/skills/prompt-engineering/` | Templates and checklists for all primitives |

## General Guidelines

- Respond in the same language the user writes in (German or English)
- Prefer targeted, minimal changes — do not refactor beyond what is asked
- When writing DAX, always validate syntax via `powerbi-model_dax_query_operations` (Validate operation)
- When creating measures or columns, use the MCP tools to apply changes directly — do not edit `.tmdl` files manually
