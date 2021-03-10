# Multi-page PDF's from web pages

## Introduction

Web pages are not split into multi pages by default, whereas PDF's are. The content of a webpage is usually responsive and adaptable to screen from 400 to 4000 pixels whereas PDF's (especially reports) are usually formatted for the A4 format (210 x 297mm).

Hence, converting a web page into a PDF which contains multiple pages is not a trivial thing to do.

The default mechanism browsers offer is based on [CSS paged media](https://developer.mozilla.org/en-US/docs/Web/CSS/Paged_Media). Futhermore, the standard is rather simple and right in the comfort zone of web developers (especially frontend developers). This means that the implementaion is straight-forward and easily maintainable. However, following are the major short-comings of the approach:

- The PDF cannot be generated directly, instead the browser's print dialog is opened after which the user ought to click on "Save"
- The footer cannot be removed from the first (cover) page
- Display page numbers in the footer (note on browser support)
- The feature is not standardized across browsers
Granted, the issues above are expected to be solved at some point in the future as browser support is improved, but there is no time frame. Additionally, it's usually Chrome who is leading the charge followed by Firefox (The Chromium Edge is now right at the heels of Chrome).

## The jsPDF and html2canvas implementation

The circumvent the problems outlined in the previous section a possible approach is to use html2canvas to generated a canvas containing a rasterized version of the web page. The image can then be embedded into a PDF created using jsPDF.

### Multi-page support

Multiple canvases may be generated based on the top level sections of the webpage. If a section is too long it may be split up into multiple sections which can fit into a single page or less.

html2canvas outputs canvas elements based on the HTML elements which represent sections. Next, each canvas is added to the current page if its height is less than the remaining space (taking page padding into account is important). In case that the current canvas is longer that the reminding space, a new page is inserted before embedding the current section.

### Units

This implementation is based on millimeters. However the width and height on a canvas element are in pixels. A solution to this problem is to convert pixels to millimeters based on the ratio between the CSS width of the sections (in millimeters) and the canvas element width in pixels.
