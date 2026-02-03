# Feature: Increase Drop Zone Surface Area

## Metadata
issue_number: `7`
adw_id: `83155f00`
issue_json: `{"number":7,"title":"Increase drop zone surface area","body":"/feature\n\nadw_sdlc_iso\n\nLets increase the drop zone surface area. Instead of having to click \"upload data\". The user can drag and drop right on to the upper div or lower div and the UI will update to a 'drop to create table' text. This runs the same usual functionality but enhances the UI to be more user friendly."}`

## Feature Description
Enhance the user interface to support drag-and-drop functionality directly on both the sample data section (upper div) and the file upload section (lower div) within the upload modal. When users drag files over these areas, the UI will provide visual feedback with "Drop to create table" text, making the upload process more intuitive and accessible. This eliminates the need to click the "Upload Data" button first and creates a more seamless user experience.

## User Story
As a user
I want to drag and drop files directly onto the sample data section or file upload section
So that I can quickly upload data without needing to click buttons, creating a more intuitive and efficient workflow

## Problem Statement
Currently, users must click the "Upload Data" button to open a modal, then either click "Browse Files" or drag-and-drop within the designated drop zone. This multi-step process creates unnecessary friction, especially for users who want to quickly upload files. The sample data section (upper div) and file upload section (lower div) are not interactive for drag-and-drop, limiting the surface area available for file uploads and reducing the overall user experience.

## Solution Statement
Make both the sample data section (upper div with class `sample-data-section`) and the file upload section (lower div with id `drop-zone`) accept drag-and-drop file uploads. When users drag files over either section:
1. Display visual feedback showing "Drop to create table" text
2. Apply hover styling to indicate the drop target is active
3. Process the dropped file using the existing upload flow
4. Maintain all existing upload functionality (validation, error handling, success messages)

This approach maximizes the interactive surface area within the modal, providing users with a larger target for drag-and-drop operations while maintaining a clean and intuitive interface.

## Relevant Files
Use these files to implement the feature:

- `app/client/index.html` (lines 59-93)
  - Contains the modal structure with upper div (`.sample-data-section`) and lower div (`#drop-zone`)
  - Lines 59-84: Sample data section (upper div) that needs drag-and-drop capability
  - Lines 86-93: Existing drop zone (lower div) with current drag-and-drop implementation
  - Both sections need to accept file drops and show appropriate visual feedback

- `app/client/src/main.ts` (lines 123-158, 161-174, 391-416)
  - Lines 123-158: `initializeFileUpload()` function with existing drag-and-drop handlers
  - Lines 161-174: `handleFileUpload()` function that processes uploaded files
  - Lines 391-416: `displayUploadSuccess()` function that handles successful uploads
  - Need to extend drag-and-drop handlers to cover the sample data section

- `app/client/src/style.css` (lines 247-263, 469-521)
  - Lines 247-263: `.drop-zone` styling with dragover state
  - Lines 469-521: `.sample-data-section` styling
  - Need to add dragover states for the sample data section to provide visual feedback
  - Need to add CSS for "Drop to create table" text overlay

- `app/client/src/api/client.ts` (lines 38-45)
  - Contains the `uploadFile()` API method used by the upload handler
  - No changes needed, but relevant for understanding the upload flow

### New Files

- `.claude/commands/e2e/test_drag_drop_enhanced.md`
  - E2E test to validate the enhanced drag-and-drop functionality
  - Tests drag-and-drop on both upper div (sample data section) and lower div (file upload section)
  - Validates visual feedback ("Drop to create table" text appears)
  - Ensures file upload works correctly from both drop zones

## Implementation Plan

### Phase 1: Foundation
Before implementing the drag-and-drop functionality, ensure understanding of the current upload flow:
- Review the existing modal structure in `index.html` (both upper and lower divs)
- Understand the current drag-and-drop implementation in `main.ts` for the drop zone
- Identify the CSS classes used for dragover states
- Confirm the file upload API flow through `handleFileUpload()` function

### Phase 2: Core Implementation
Extend the drag-and-drop functionality to both sections:
- Add drag event listeners to the sample data section (upper div with class `.sample-data-section`)
- Create visual feedback mechanism to display "Drop to create table" text on dragover
- Apply consistent styling across both drop zones when files are dragged over
- Ensure proper event handling (preventDefault, stopPropagation) to avoid conflicts
- Integrate with existing `handleFileUpload()` function to maintain consistent behavior

### Phase 3: Integration
Connect the new functionality with existing systems:
- Test file drop on both sections triggers the same upload flow
- Verify modal behavior remains consistent (closes on successful upload)
- Ensure error handling works for invalid file types
- Validate that sample data buttons still work independently
- Test edge cases (drag over then drag away, multiple files, invalid file types)

## Step by Step Tasks
IMPORTANT: Execute every step in order, top to bottom.

### Task 1: Create E2E Test File
- Read `.claude/commands/test_e2e.md` to understand the E2E test runner structure
- Read `.claude/commands/e2e/test_basic_query.md` as an example E2E test
- Create `.claude/commands/e2e/test_drag_drop_enhanced.md` with test steps:
  - Navigate to application
  - Open upload modal
  - Verify both sample data section and drop zone are visible
  - Simulate drag event over sample data section (upper div)
  - Verify "Drop to create table" text appears
  - Simulate drop event with a CSV file on sample data section
  - Verify file upload succeeds and table is created
  - Repeat test for drop zone (lower div)
  - Take screenshots at each key step

### Task 2: Add CSS for Enhanced Drop Zones
- Open `app/client/src/style.css`
- Add a new CSS class `.drop-zone-active` for the dragover state
- Add styling for `.sample-data-section.drop-zone-active` to show visual feedback
- Add a `.drop-overlay` class for displaying "Drop to create table" text:
  - Position: absolute overlay on the dragged-over section
  - Background: semi-transparent with primary color tint
  - Text: centered, large font size, white color
  - Pointer events: none (to allow drop event to pass through)
- Ensure the styling is consistent with existing `.drop-zone.dragover` styling

### Task 3: Update HTML Structure for Drop Overlay
- Open `app/client/index.html`
- Add `position: relative` wrapper or ensure both `.sample-data-section` and `#drop-zone` can contain absolutely positioned overlays
- Consider adding data attributes to identify droppable sections (e.g., `data-drop-target="true"`)

### Task 4: Extend Drag-and-Drop Event Handlers
- Open `app/client/src/main.ts`
- Locate the `initializeFileUpload()` function (lines 123-158)
- Create a new helper function `addDropZoneHandlers(element: HTMLElement)` that:
  - Adds `dragover` event listener: prevents default, adds visual feedback class, shows "Drop to create table" text
  - Adds `dragleave` event listener: removes visual feedback class, hides overlay text
  - Adds `drop` event listener: prevents default, removes visual feedback, extracts file, calls `handleFileUpload()`
- Apply `addDropZoneHandlers()` to both:
  - The existing drop zone element (`#drop-zone`)
  - The sample data section element (`.sample-data-section`)

### Task 5: Create Drop Overlay Element Dynamically
- In `main.ts`, create a function `createDropOverlay()` that:
  - Creates a div element with class `.drop-overlay`
  - Sets inner text to "Drop to create table"
  - Returns the element
- In `addDropZoneHandlers()`, on `dragover`:
  - Check if overlay already exists in the target element
  - If not, create overlay using `createDropOverlay()` and append to target
  - Show the overlay
- On `dragleave` and `drop`:
  - Remove the overlay element from the target

### Task 6: Handle Edge Cases
- Ensure `dragleave` event correctly identifies when the mouse leaves the drop zone (not triggered by child elements)
- Use a counter or `relatedTarget` check to prevent flickering when dragging over child elements
- Validate that only accepted file types (.csv, .json, .jsonl) trigger the upload
- Show appropriate error messages for invalid file types

### Task 7: Test Manual Upload Flow
- Start the application using `./scripts/start.sh`
- Test the following scenarios manually:
  - Drag a .csv file over the sample data section (upper div) - verify "Drop to create table" appears
  - Drop the file - verify upload succeeds
  - Drag a .json file over the drop zone (lower div) - verify "Drop to create table" appears
  - Drop the file - verify upload succeeds
  - Drag an invalid file type - verify error message displays
  - Drag over and drag away without dropping - verify overlay disappears correctly
  - Verify sample data buttons still work independently

### Task 8: Run Validation Commands
- Execute the `Validation Commands` section below to ensure zero regressions
- Fix any issues that arise during validation
- Re-run validation until all tests pass

## Testing Strategy

### Unit Tests
While this feature is primarily UI-focused and will be validated through E2E tests, consider the following areas for potential unit testing if the codebase has a unit testing framework:
- File type validation logic (if extracted into a separate function)
- Drop overlay creation and removal logic
- Event handler attachment and cleanup

### Edge Cases
Test the following edge cases during manual and E2E testing:
1. **Invalid File Types**: Drag and drop a .txt or .exe file - should show error message
2. **Multiple Files**: Drag multiple files at once - should only process the first file (or show appropriate message)
3. **Drag Over Child Elements**: Drag over nested elements within the drop zone - overlay should not flicker
4. **Drag Away Without Dropping**: Drag over the drop zone then move away - overlay should disappear
5. **Rapid Drag Events**: Quickly drag in and out multiple times - should handle gracefully without visual glitches
6. **Modal Close During Drag**: If modal closes while dragging, ensure no orphaned event listeners
7. **Successful Upload**: After dropping a file, verify modal closes and success message displays
8. **Existing Browse Button**: Verify the "Browse Files" button still works after adding drag-and-drop to sample section
9. **Sample Data Buttons**: Verify sample data buttons still function independently without interference
10. **Large Files**: Test with a large CSV file to ensure loading states appear correctly

## Acceptance Criteria
- [ ] Users can drag and drop files onto the sample data section (upper div)
- [ ] Users can drag and drop files onto the file upload section (lower div)
- [ ] When dragging files over either section, "Drop to create table" text appears as a visual overlay
- [ ] The overlay disappears when the user drags away without dropping
- [ ] Dropping a valid file (.csv, .json, .jsonl) triggers the upload flow
- [ ] Invalid file types show an appropriate error message
- [ ] The modal closes automatically after a successful upload
- [ ] Sample data buttons continue to work independently
- [ ] The "Browse Files" button continues to work as before
- [ ] Visual feedback is consistent with the existing design language
- [ ] No regressions in existing upload functionality
- [ ] All validation commands pass without errors

## Validation Commands
Execute every command to validate the feature works correctly with zero regressions.

1. Read `.claude/commands/test_e2e.md` to understand how to run E2E tests
2. Read and execute `.claude/commands/e2e/test_drag_drop_enhanced.md` to validate the enhanced drag-and-drop functionality works correctly on both upper and lower divs
3. `cd app/server && uv run pytest` - Run server tests to validate the feature works with zero regressions
4. `cd app/client && bun tsc --noEmit` - Run frontend type checking to validate no TypeScript errors
5. `cd app/client && bun run build` - Run frontend build to validate the feature works with zero regressions

## Notes
- The feature enhances UX by making the entire modal more interactive for file uploads
- Maintain consistent visual feedback across both drop zones to avoid user confusion
- Consider accessibility: ensure screen readers can understand the drop zones (may need ARIA labels)
- The existing upload flow (`handleFileUpload` → API call → success/error handling) should not be modified, only the trigger points (drag-and-drop on additional elements)
- If future features require disabling drag-and-drop, consider adding a data attribute or class to toggle the functionality
- Performance consideration: creating/destroying overlay elements on each drag should be lightweight, but monitor for any lag on slower devices
- Consider adding a subtle animation for the overlay appearance to enhance the user experience
- The "Drop to create table" text should be clear and actionable, matching the application's tone
