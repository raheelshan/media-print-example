# Print DOM as Image with Print-Specific CSS

This small project captures part of a webpage as an image using `html2canvas` and opens it in a new tab for printing. A print-specific CSS file (`style.css`) is loaded only in the print window to hide elements and format the output.

## Features

- Convert a specific DOM element into an image.
- Open a new tab with only the captured image.
- Load `style.css` dynamically **only during print**.
- `style.css` includes `@media print` rules to:
  - Hide the original layout.
  - Center and scale the printed image.
