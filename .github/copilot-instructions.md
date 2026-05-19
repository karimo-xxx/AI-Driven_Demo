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

## DAX Query Result Display

When executing DAX queries, always present results in this format:

1. **Markdown table** — Results as a formatted table with thousands separators and decimal places
2. **DAX query in a code block** — Show the executed query as a `dax` code block
3. **Brief interpretation** — One or two sentences summarizing the results

Example:

| Fiscal Year | Total Sales Amount |
|-------------|-------------------|
| FY2018 | 23.860.891,17 |

```dax
EVALUATE
SUMMARIZECOLUMNS(...)
```

> Number format: German locale — dot as thousands separator, comma as decimal separator.

### Accessing DAX Result Files

MCP tool resource URIs are often inaccessible after execution. The actual CSV results are stored at:

```
%LOCALAPPDATA%\Temp\PowerBIModelingMCP\QueryResults\dax_query_result_<timestamp>.csv
```

After `Execute`, read the most recent CSV file from this directory (sorted by `LastWriteTime`) to reliably display results.

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
Adventure Works DW 2020.Report/
Adventure Works DW 2020.SemanticModel/
```

## General Guidelines

- Respond in the same language the user writes in (German or English)
- Prefer targeted, minimal changes — do not refactor beyond what is asked
- When writing DAX, always validate syntax via `powerbi-model_dax_query_operations` (Validate operation)
- When creating measures or columns, use the MCP tools to apply changes directly — do not edit `.tmdl` files manually
