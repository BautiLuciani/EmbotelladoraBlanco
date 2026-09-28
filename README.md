# Leads Sheet Cleaner: n8n Workflow

An n8n workflow that automatically cleans a Google Sheets leads list by removing records with invalid phone numbers and normalizing the valid ones.

Built as part of a Facebook Messenger lead-capture setup, where leads are collected in a spreadsheet and need to be kept clean for follow-up.

## What it does

1. **Schedule Trigger** starts the workflow on a recurring interval.
2. **Get row(s) in sheet** reads all rows from the `Leads` sheet.
3. **Filtrar y agrupar registros** (Code node) validates each lead's phone number:
   - Discards empty values.
   - Discards values containing letters or invalid characters (e.g. "no data").
   - Discards numbers with fewer than 8 digits.
   - Normalizes valid numbers to digits only (removes `+`, spaces, dashes, dots and parentheses).
   - Outputs both the valid and the discarded records, with counts and the discard reason.
4. **Clear sheet** wipes the sheet while keeping the header row.
5. **Recuperar registros válidos** (Code node) unpacks the valid records into individual items.
6. **Append row in sheet** writes the clean records back to the sheet.

```
Schedule Trigger → Get rows → Filter & group → Clear sheet → Unpack valid → Append rows
```

## Sheet columns

`ID`, `Nombre`, `Telefono`, `Direccion`, `Localidad`, `Comentarios`, `Tipo Promo`, `Promoción Interes`, `FechaRegistro`, `FechaUltimoMensaje`

## Tech

- n8n
- Google Sheets (OAuth2)
- JavaScript (n8n Code nodes)

## How to use

1. Import `leads-sheet-cleaner.json` into n8n (**Workflows → Import from file**).
2. Replace `YOUR_SHEET_ID` with your own Google Sheet ID in the three Google Sheets nodes.
3. Assign your own Google Sheets OAuth2 credential.
4. Set the schedule interval in the Schedule Trigger node.
5. Activate the workflow.

## Notes

- Identifiers (sheet ID, credential ID, instance ID) were replaced with placeholders before publishing.
- The workflow clears and rewrites the sheet, so test it on a copy first. Discarded records are not saved anywhere by default; a possible improvement is to log them to a separate sheet before clearing.
