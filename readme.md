# Slide Instructions

This project uses mkdocs, kobo, and some javascript to make an educational or project decision workflow where output is saved in kobo.  It uses a simplified "Configuration Div" system to handle slide logic (forms, redirects, images, and email persistence). Instead of writing complex HTML/JS in every slide, you simply place one configuration line at the bottom of your whatever.md file in docs.

- The email from the email setting form is preserved into the next pages and shows up automatically in the kobo forms if you do things correctly, and the user only submits forms or uses the "go to the next page" button at the bottom.  The javascript takes care of that.  
- If they navigate using the navigation menu on the left side of the screen, the email persistance may not persist.
- You can see it as /?email=a@b.c at the end of the url in your browser

In theory, you should be able to set up a new github mkdocs repo and copy this stuff over, and then edit it to what you want.  

- You will also need a kobo account.

- There are also fancy github actions repositories that house some figures that are generated based on people's responses for some use cases, but this documentation does not cover that.

## Stuff inside

- the docs folder has the text that people will see, with each page being a file named something.md as well as the image files that these call.
  - inside of the docs folder is the stylesheets and javascripts folders, which has the code that performs navigation and remembers emails
  - the javascripts folder might be moved to its own repository in the future, to keep things more consistent, but more complex
- the koboforms folder has the kobo forms that were used and uploaded into kobo for the workflow
- the site folder is automatically generated so you dont need to worry about that
- the mkdocs.yml file has
    - the navigation flow on the left side of the screen.  You need to type in the -'filename.md' in order for each page for it to work.
    - settings for things like formatting, and where the javascript is

## How to Use

- You make a name.md markdown file for each page of your workflow.
- the first page should be called index.md
- make sure to edit the mkdocs.yml file to include each page in the workflow.
  - This will be under the nav: line and look something like - 'afilename.md'
  - if you dont do it, most things will work, but the side navigation bar will be broken
- At the bottom of each Markdown file, you send some information to javascript in a single line telling it what you want.  It can be broken into multiple lines if you like that.
  - An example from the index.md, which simply gets peoples emails for future slides is:
``` <div id="slide-config" data-type="start" data-next="../disastermandate/"> </div> ```
- **Important: You need to tell it the name of your next slide in that line**
  - That means you have to put the slide order both in mkdocs.yml and one at a time in the '<div' part of the md file.

### Start Slide (Email Entry)

The entry point. Asks the user for their email, saves it to session storage, and forwards it to the next slide.

Example md in this repo: index.md

Attributes:

data-type="start"

data-next: Path to the first real content slide.

Example:
'''# Beginning
Lets start!

<div id="slide-config" 
     data-type="start" 
     data-next="../slide1/">
</div>
'''
### Kobo Form Slide

Displays a KoboToolbox form. It automatically prefills the user's email (if captured previously) and handles the redirect upon submission.  

Some kobo notes (deeper documentation needed).
- best way to generate a kobo form is to copy and the xlsx files in the koboforms folder
  - then go to kobo -> forms, click on new, click on upload xlsx form and drag your form there.
  - Sector is "Education", country is "United States"
  - **Make sure "Allow submissions to this form without a username and password
" is switched ON**
  - click "DEPLOY"
  - click on the "open" button near the bottom. Then you can look at the URL. https://ee.kobotoolbox.org/x/YourID. That gives you the id for data-kobo-id="YourID"


Example file in this repo: game_time.md

Attributes:

data-type="kobo"

data-next: Path to the next slide (e.g., ../slide2/ or slide2.html).

data-kobo-url: The URL of your Kobo form (e.g., https://ee.kobotoolbox.org/x/YourID).

Example:

'''# Questions

Fill out this form

<div id="slide-config" 
     data-type="kobo" 
     data-next="../slide3/" 
     data-kobo-id="b5CVmfVW">
</div>
'''
### Figure Slide

Displays an image and a "Next" button. It automatically adds a timestamp to the image URL to force a fresh reload (preventing stale cache issues).

Example file in this repo: peoplesinvestments.md

Attributes:

data-type="figure"

data-next: Path to the next slide.

data-img: Path to your image file (e.g., assets/chart.png, but it can even be a url).

Example:
'''# Results

Here is what people put in the forms:

<div id="slide-config" 
     data-type="figure" 
     data-next="../slide4/" 
     data-img="https://dosgoodcu.github.io/auto-assets/public_investment_chart.png">
</div>
'''
Simple Text Slide

Displays standard Markdown text with a "Next" button at the bottom.

Example in this repo: savingspoints.md

Attributes:

data-type="simple"

data-next: Path to the next slide.

Example:
'''# Information
Read this important text. Click the button below when you are done.

<div id="slide-config" 
     data-type="simple" 
     data-next="../slide2/">
</div>
'''

### Final Slide

Only put the markdown you want, no code.

Example in this repo: bye.md

Example:
'''#Bye

Thanks!
'''

### Helpful Markdown
Other helpful markdown that works in this environment:

-Simplest figure:
'''![](hat_gum.png)'''

-Fancier figure (if you need to control size)
'''<img src="hat.jpg" alt="Hat" style="width:500px;">'''

-Embedded url window (eg a maptool that is active):
'''<div style="text-align: center; margin-top: 10px;">
    <iframe id="resizableFrame"
        src="https://fist.iri.columbia.edu/publications/docs/Madagascar_AA_FLexDashboard_OND_2024_FR/"
        width="1200" height="1300"
        style="border:1px solid black; transition: all 0.3s ease;"></iframe>
</div>'''



