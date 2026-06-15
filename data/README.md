# Excel Intake Template

Place your `Master_Input_Data_Template.xlsx` file in this folder for version control.

## Sheet Structure

Each of the 3 sheets must:
1. Be formatted as an **Excel Table** (Insert → Table)
2. Have exactly two columns: `Category` and `Current State`
3. Be named exactly as listed below (n8n node references these names)

## Required Sheet Names
- `Industry & Scale`
- `Tech Stack & Data Readiness`  
- `Governance Posture`

## Adding New Data Points
Simply add a new row with the field name in `Category` and the value in `Current State`.
The `kvToObject()` JavaScript function in the Code node reads all rows dynamically.

## Note on shared_context fields
The following fields from Sheet 1 are extracted as `shared_context` and passed to ALL agents.
If you rename these fields in the Excel, update the corresponding lines in the Code node:
- Company
- Industry
- Sub sector
- Employees
- Revenue
- Geography
- AI budget
- Maturity Level
- Organizational Capacity
