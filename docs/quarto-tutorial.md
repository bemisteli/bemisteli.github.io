# Building a Personal Research Website with Github Pages and Quarto 


## Why build a website as an academic or researcher?

 Hosting a research website is a great way to present yourself professionally as a scientist, aggregate your contact information, and network with potential collaborators. It can act as a portfolio for your projects and help you communicate your research to a wider audience. 
 
 You can do things like:
 
* Share links to data and code repositories
* Aggregate information (publications, resources, contact info) in one place
* Connect with potential collaborators
 
## Quarto 

[Quarto](https://quarto.org/) is a self-described open source scientific and technical publishing system. Quarto is an awesome website choice for those already familiar with [Markdown syntax](https://www.markdownguide.org/basic-syntax/) and R or Python. Quarto websites are (1) free and customizable and (2) come together surprisingly quickly.



#### Example: My Quarto site  [alburycatalina.github.io](https://alburycatalina.github.io/)


#### How?
Create a site with Rstudio's built in Quarto functions. Host the site with Github Pages and render the content with Github actions in a CI/CD workflow. 

Note: I am using a mac and bash to run commands. You may need to modify these slightly if using a different terminal. 

# Let's Get Started. You will need:
- [Github account](https://github.com/) and git
- Quarto
- IDE of your choice (Rstudio or VSCode are two personal favorites)
- A browser to view your site in construction


# Step 1: Setup
**1. [Install git on your machine](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)**

Git is a SCM (source code management) software. You can use it to track and finalize changes in code. Github is a place to store this version control information on the internet. Git is installed by default on most Mac and Linux machines. 

**2. [Install Quarto](https://quarto.org/docs/get-started/).**

Follow the installation requirements on the [Quarto site](https://quarto.org/docs/get-started/). 


# Step 2: Make a New Github Repository (AKA "repo") 

**1. Make a github repo to host your site**

You have two options for the URL that your website will be hosted under: either (your username).github.io or (your username).github.io/(repository name). Either is great depending on your usage, but let's try out hosting via your main URL for now. 
  
Name the new repository (your username).github.io. You should tick the "add a README file" box and select whether your repository will be public or private (deploying Github Pages from private repos is restricted to Github Pro, which available at no cost to students). 
 
**2. Clone the Repo**

Once you've created the repository, you can [clone it to your computer](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository) with command line. This creates a local copy of the repo on your computer. 
 
Hit the green code button in the top right corner of the github repository and copy the link to clone with HTTPS.
 
 Make a folder for your website (ex: a folder called "Personal_Website" and change the terminal directory to it.
 
 In terminal, type `cd ~/Personal_Website`
 
 Type `git clone` and paste in the copied URL from before. 
 
 For example: `git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY`
 
 If you open the folder that you cloned your repo to, you should see the README file. 
  
  
  
# Step 3: Make a Site
Quarto has great documentation on getting started [here](https://quarto.org/docs/websites/). 

Start by creating a new R project in Rstudio in your Git repo's folder. Select 'Quarto Site'. You can write Markdown syntax here and hit the render button to generate a preview of your site which can be opened in the browser. See the repo below for more info.

There are 2 basic components to a Quarto site: 

`index.qmd`: The first site that visitors will see when they visit your site. All other pages will link from here. Contains markdown syntax for content. It also contains a "front matter" section denoted by three dashes. The front matter contains formatting instructions for the page. 

``` {.yaml}
# front matter here
```

`quarto.yml` contains formatting instructions for the rest of the document. A `quarto.yml` file will be automatically created for you when you start your Quarto site. It will be pretty empty, so you can pull inspiration from other's for your setup. 

Here you can see an example from the Quarto documentation that I've annotated.


``` {.yaml filename="_quarto.yml"}
project:
  output-dir: _output # place files from render in the _output folder

toc: true # create a table of contents
number-sections: true # number the sections
bibliography: references.bib  # include a bibliography file
  
format:
  html:
    css: styles.css # pull css styling from styles.css
    html-math-method: katex
```

Now you've got a site! It's plain right now but you'll make it yours soon enough. 

# Step 4: Customize

There are many website examples on the [Quarto Gallery](https://quarto.org/docs/gallery/#websites) for you to borrow ideas from and remix. 

You can also try creating a bilingual site with Quarto profiles [as described by Mario Angst](https://quarto-dev.marioangst.com/en/blog/posts/multi-language-quarto/). 

# Step 5: Publish!

When the website is complete, host it on your Github page and share the link with your friends, family, colleagues, and enemies. 

You can do this by following the instructions on the Quarto site [here](https://quarto.org/docs/publishing/github-pages.html#github-action). I prefer to deploy from a Github Action. If you're running a multilingual site, [rendering to docs is best](https://quarto.org/docs/publishing/github-pages.html#render-to-docs). 

If you have any code that you'd like rendered on your site, add the following code to your `quarto.yml` to create a _freeze directory that stores the output of your locally computed code. 

``` {.yaml}
execute:
  freeze: auto
```

Run `quarto render` to render your site a _freeze folder should appear. 

Run `quarto publish gh-pages` 

Create the instructions for the Github action by creating a .github folder and adding the following code at `.github/workflows/publish.yml`. 

``` {.yaml}
on:
  workflow_dispatch:
  push:
    branches: main

name: Quarto Publish

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Check out repository
        uses: actions/checkout@v4

      - name: Set up Quarto
        uses: quarto-dev/quarto-actions/setup@v2

      - name: Render and Publish
        uses: quarto-dev/quarto-actions/publish@v2
        with:
          target: gh-pages
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```
This will publish your site everytime you publish to main. 

In the Pages settings of your Github repository ensure that the Source branch for your repository is gh-pages and that the site directory is set to the repository root (/). 


If done correctly, when you push to main you should see the deployment working in the Actions section of the repo. Navigate to (your username).github.io to see the site. 
**Successs!**

It can take a few minutes for changes in your Github repo to be reflected on the live page. 


# Step 6: Stage, Commit, and Push to Update

In the future, edit website files in the IDE of your choice and use the following commands in terminal to stage, commit, and push  edits to the Github directory. 

-`git add (file name)` Stages the named file for commit. You can modify this command to only change certain files (ex: `git add myfolder`). 

-`git commit -m "Message that describes what this change does"` Gives you a spot to comment on the change you made

-`git push -u origin main` "Pushes" the final changes to Github in the main branch. 

Enjoy your new website :-)







