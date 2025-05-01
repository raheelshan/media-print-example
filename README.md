# Print Content of HTML without using Media Queries

This small project captures part of a webpage as an image using `html2canvas` and `jsPdf`.

## Usage

1. The HTML Structure

```
<body>
    <div id="root-container">
        <!-- html content -->
        <div id="content-to-print">
            <!-- add your content here -->
        </div>
        <!-- other content -->
    </div>  
     <!-- empty image element to hold converted image from DOM -->
    <img src="" alt="" id="printed-image" />
</body>
```

2. Convert HTML Content to Image

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.4.0/jspdf.umd.min.js"></script>
<script>
// generate image on document ready to be saved in image tag
$(document).ready(function() {
    generateImage()
})

// function to be called on button click
function printImage() {
    window.print();
}

// logic to convert content to image
function generateImage() {

    const style = document.createElement('style');
    document.head.appendChild(style);
    style.sheet?.insertRule('body > div:last-child img { display: inline-block; }');

    html2canvas(document.getElementById('content-to-print'), {
        useCORS: true, // To handle cross-origin images
        scale: 3,
        backgroundColor: '#FFF', // Do not force a background color in html2canvas
        scrollX: 0,
        scrollY: 0,
        windowWidth: document.documentElement.scrollWidth,
        windowHeight: document.documentElement.scrollHeight
    }).then(function(canvas) {
        const {
            jsPDF
        } = window.jspdf;
        const imgData = canvas.toDataURL('image/png');
        const pdf = new jsPDF('p', 'mm', 'a4'); // 'p' for portrait, 'mm' for millimeters, 'a4' for A4 size
        const imgWidth = 210; // A4 width in mm (210mm)
        const pageHeight = 285; // A4 height in mm (297mm)
        const imgHeight = canvas.height * imgWidth / canvas.width;
        const heightLeft = imgHeight;
        let position = -7;

        // Add a white background to the PDF page
        pdf.setFillColor(255, 255, 255); // RGB for white
        pdf.rect(0, 0, imgWidth, pageHeight, 'F'); // Fill the background

        pdf.addImage(imgData, 'PNG', 0, position, imgWidth, imgHeight);

        $('#printed-image').prop('src', imgData)
    });
}
</script>  
```

3. CSS for Print

```
/* Default styles */
#printed-image {
    display: none;
}

/* Print-specific styles */
@media print {
    #root-container {
        display: none !important;
    }

    #printed-image {
        display: block !important; /* Force image to display only during print */
        margin: auto;             /* Optional: center the image */
        max-width: 100%;          /* Ensure image fits the page */
    }
}
```