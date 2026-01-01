# Patch: CSV Button Text Match

## Metadata
adw_id: `64dce0bf`
review_change_request: `bug - adw_patch_iso - Under available tables section the csv export button has the correct text '📊CSV'. Update the query result section csv export button text should match this.`

## Issue Summary
**Original Spec:** None provided
**Issue:** The CSV export button in the query results section displays "📊 CSV Export" while the available tables section displays "📊 CSV". The text should be consistent across both sections.
**Solution:** Update the query results CSV button text to display only "📊 CSV" without the " Export" suffix to match the available tables section format.

## Files to Modify
Use these files to implement the patch:

- `app/client/src/main.ts` - Line 242: Update export button innerHTML

## Implementation Steps
IMPORTANT: Execute every step in order, top to bottom.

### Step 1: Update query results CSV button text
- Locate line 242 in `app/client/src/main.ts`
- Change `exportButton.innerHTML = \`${getDownloadIcon()} Export\`;` to `exportButton.innerHTML = getDownloadIcon();`
- This removes the " Export" text suffix to match the available tables section button format

## Validation
Execute every command to validate the patch is complete with zero regressions.

1. **TypeScript Type Check**: `cd app/client && bun tsc --noEmit`
2. **Frontend Build**: `cd app/client && bun run build`
3. **Python Syntax Check**: `cd app/server && uv run python -m py_compile server.py main.py core/*.py`
4. **Backend Code Quality Check**: `cd app/server && uv run ruff check .`
5. **All Backend Tests**: `cd app/server && uv run pytest tests/ -v --tb=short`

## Patch Scope
**Lines of code to change:** 1
**Risk level:** low
**Testing required:** Visual verification that CSV button text matches between query results and available tables sections, plus automated test suite validation
