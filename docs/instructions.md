Slide Manager Instructions

This project uses a simplified "Configuration Div" system to handle slide logic (forms, redirects, images, and email persistence). Instead of writing complex HTML/JS in every slide, you simply place one configuration line at the top of your Markdown file.

1. Installation

Ensure the following files are in place:

JavaScript: docs/javascripts/slide-manager.js

CSS: docs/stylesheets/styles.css

Configuration: Update mkdocs.yml:

extra_css:
  - stylesheets/styles.css

extra_javascript:
  # kobo_loader.js is retained for backwards compatibility
  - javascripts/kobo_loader.js
  - javascripts/slide-manager.js


2. How to Use

At the top of your Markdown file (below the YAML frontmatter if you use it), add a single HTML <div> with the ID slide-config. The JavaScript reads this line to determine what to render.

Type 1: Kobo Form Slide

Displays a KoboToolbox form. It automatically prefills the user's email (if captured previously) and handles the redirect upon submission.

Attributes:

data-type="kobo"

data-next: Path to the next slide (e.g., ../slide2/ or slide2.html).

data-kobo-url: The URL of your Kobo form (e.g., https://ee.kobotoolbox.org/x/YourID).

Example:

<div id="slide-config" 
     data-type="kobo" 
     data-next="../slide3/" 
     data-kobo-url="[https://ee.kobotoolbox.org/x/aB1cD2eF](https://ee.kobotoolbox.org/x/aB1cD2eF)">
</div>

# Survey
Please complete the form below.


Type 2: Figure Slide

Displays an image and a "Next" button. It automatically adds a timestamp to the image URL to force a fresh reload (preventing stale cache issues).

Attributes:

data-type="figure"

data-next: Path to the next slide.

data-img: Path to your image file (e.g., assets/chart.png).

Example:

<div id="slide-config" 
     data-type="figure" 
     data-next="../slide4/" 
     data-img="https://dosgoodcu.github.io/auto-assets/public_investment_chart.png">
</div>

# Results
Here is your latest performance graph.


Type 3: Simple Text Slide

Displays standard Markdown text with a "Next" button at the bottom.

Attributes:

data-type="simple"

data-next: Path to the next slide.

Example:

<div id="slide-config" 
     data-type="simple" 
     data-next="../slide2/">
</div>

# Information
Read this important text. Click the button below when you are done.


Type 4: Start Slide (Email Entry)

The entry point. Asks the user for their email, saves it to session storage, and forwards it to the next slide.

Attributes:

data-type="start"

data-next: Path to the first real content slide.

Example:

<div id="slide-config" 
     data-type="start" 
     data-next="../slide1/">
</div>

# Welcome
Please enter your email to begin the simulation.


3. Key Features & Troubleshooting

Email Persistence: The system checks both the URL (?email=...) and Session Storage. This ensures the email variable is never lost between slides.

Frame Busting: If a slide is loaded inside a small iframe (common with Kobo redirects), the script automatically forces the top-level window to navigate to the slide.

Backward Compatibility: This system runs alongside kobo_loader.js. Old slides using <script>loadKoboForm(...)</script> will continue to work, but new slides should use the slide-config method.
