# E2E Test: Enhanced Drag and Drop Functionality

Test enhanced drag-and-drop functionality for file uploads in the Natural Language SQL Interface application.

## User Story

As a user
I want to drag and drop files directly onto the sample data section or file upload section
So that I can quickly upload data without needing to click buttons, creating a more intuitive and efficient workflow

## Test Steps

### Part 1: Test Drag-Drop on Sample Data Section (Upper Div)

1. Navigate to the `Application URL`
2. Take a screenshot of the initial state
3. **Verify** the page title is "Natural Language SQL Interface"
4. **Verify** the "Upload Data" button is visible
5. Click the "Upload Data" button to open the modal
6. Take a screenshot of the opened modal
7. **Verify** the sample data section (upper div) is visible
8. **Verify** the file upload section (lower div with drop zone) is visible

9. Simulate drag event over the sample data section (upper div):
   - Use JavaScript to dispatch dragenter and dragover events on the `.sample-data-section` element
   - **Verify** "Drop to create table" text appears as an overlay
   - Take a screenshot showing the drag overlay on sample data section

10. Simulate dragleave event:
    - Use JavaScript to dispatch dragleave event
    - **Verify** the overlay disappears

11. Create a test CSV file in memory with sample data
12. Simulate drop event with the CSV file on the sample data section:
    - Use JavaScript to dispatch drop event with file data
    - **Verify** file upload is triggered
    - **Verify** success message appears or modal closes
    - Take a screenshot of the success state

### Part 2: Test Drag-Drop on Drop Zone (Lower Div)

13. If modal closed, click "Upload Data" button again to reopen
14. Take a screenshot of the reopened modal

15. Simulate drag event over the drop zone (lower div):
    - Use JavaScript to dispatch dragenter and dragover events on the `#drop-zone` element
    - **Verify** "Drop to create table" text appears as an overlay
    - Take a screenshot showing the drag overlay on drop zone

16. Simulate dragleave event on drop zone:
    - Use JavaScript to dispatch dragleave event
    - **Verify** the overlay disappears

17. Create another test CSV file with different sample data
18. Simulate drop event with the CSV file on the drop zone:
    - Use JavaScript to dispatch drop event with file data
    - **Verify** file upload is triggered
    - **Verify** success message appears or modal closes
    - Take a screenshot of the final success state

### Part 3: Edge Cases

19. If modal closed, click "Upload Data" button again
20. **Verify** sample data buttons still work:
    - Click one of the sample data buttons (e.g., "Load Product Data")
    - **Verify** sample data loads correctly
    - Take a screenshot of loaded sample data

21. **Verify** the "Browse Files" button is still functional

## Success Criteria
- Sample data section (upper div) accepts drag and drop files
- Drop zone (lower div) accepts drag and drop files
- "Drop to create table" overlay appears on dragover for both sections
- Overlay disappears on dragleave for both sections
- File uploads are processed correctly from both drop zones
- Success messages display after upload
- Sample data buttons continue to work independently
- Browse Files button continues to work
- At least 7 screenshots are taken

## Notes
- Use Playwright's JavaScript evaluation to simulate drag events since browser automation may not fully support native drag-and-drop
- Create test CSV files using JavaScript Blob API or DataTransfer API
- Allow sufficient time for async operations (file uploads, API calls)
- Validate that the existing upload flow is not broken by the enhancement
