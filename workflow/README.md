# n8n Workflow Export Instructions

## How to Export

1. Open your workflow in n8n
2. Click the **three dots (⋯)** in the top-right corner of the canvas
3. Select **Download**
4. Save the file as `n8n_workflow.json` in this folder
5. Commit to GitHub

## How to Import

1. In n8n: top-right (⋯) → **Import from file**
2. Select `n8n_workflow.json`
3. Reconfigure credentials (Anthropic, Microsoft Excel 365, PDFBolt) — credentials are not exported for security reasons
4. In each Excel (Get rows) node: reselect your Workbook, Sheet, and Table

## What IS exported
- Node configurations and system prompts
- Workflow structure and connections
- All expressions and JavaScript code

## What is NOT exported
- API credentials (must be re-entered)
- Execution history
- OneDrive file references (must be reselected)
