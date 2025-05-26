# Nquerques Website Readme

## How to Use the Hugo Site

### Generate the Site and Host Locally  

***hugo server -D --config config.local.toml***  

Running this command in project root will generate and host the website locally at localhost:1313  
The website will be generated in /public  

-D means include draft posts

--config specifies the config file to use.
- config.local.toml sets the baseURL to http://localhost:1313
- config.github.toml sets the baseURL to https://nickquerques.github.io/nquerques/
- hugo site generation options can be adjusted for github pages in /.github/hugo.yml under 'name: Build with Hugo' 'run: |'


### Create Site Content

In the root of the project run

***hugo new posts/EXAMPLE.md***

this will create a new content file in the posts directory called EXAMPLE with some simple metadata.  

After the post is create you can add tags, change draft status false/true, assign type/layout.  

After the metadata you can write your post in markdown.

You can also simply create md file in content directories and manually write the metadata.

### Notes on content directories

Folders in /content are called Sections in Hugo.

_index.md will is page for the root of the content folder. See the resume content folder for example.

With no _index.md, I believe a lists.html page will be generated.

The /resume/_index.md has the metadata tags  

type: "resume"  
layout: "resume"  

This should force _index.md to use resume.html as it's source for the main block in baseof.html. 

## Theme 'mytheme'
The entire website bones are put together in the theme. The theme is just called mytheme.  

**themes/mytheme**  > This is where the theme is located. 

## Theme Directories + Files

**config.toml** > this is the overall config file for the hugo site. This is where you can title your site, assign the theme, assign the baseURL (very important for referencing different parts of the website using relURL and absURL later).  

This is also where you define all the different menu items that generate the different pages. Right now the site just has a main menu - you can see the different pages that will be generated under each [[menu.main]] entry.  

Here you can also define site parameters [params] that can be used to reference in other parts of the site using .Site.Params.ITEM.PARTOFITEM - example .Site.Params.Author.email will return nick@querques.com

**theme.toml** similar to config.toml but for the theme. I haven't played with this file much, I imagine it can be referenced using Hugo tags like config.toml

### archetypes
This directory contains the default for content pages of corresponding archetype.  

Currently I only have a default archetype for all pages. It simple includes basic metadata - the page title, publish date, and draft status (true/false).  

This could be updated in the future for specific pages types such as game/movie reviews, recipes, code projects, etc. 


### layouts/

This folder contains all of the files that make up individual pages. It also contains homepage and 404 page.
  
**index.html** > This is the homepage of the website. Needs some work.  

**404.html** > The 404 page.

### _default/

The html files in here define how every page is made.  
Pages are made up of partials and the main block.  
- Partials are defined in /partials  
- Main blocks are defined here, in /_default
  
**baseof.html** > the bones for every page on the site. Pulls the partials and main block to build a page. The main block is generated depending on the type of page. the rest of the html files in _default are different pages way of generating the main block.

#### Main block generators

**list.html**  >  this generates a main block that list content pages. example is the Posts section

**resume.html**  > because this is named resume.html and the section/type of my resume content is also called resume, hugo uses this file to generate the main block for the resume page.

**single.html** > The actual content page main block generation. 

### partials/

This folder contains the partial parts that are included in every page. There is probably a way to prioritize certain partials for specific pages.  

the html files are self-explanatory:  
**footer.html** is the page footer  
**head.html** is <head> css links, font links, site title, etc.  
**header.html**  is the actual page header. This is the navbar at the top of the page.  
**metadata.html** This i am not sure of. Looks like it prints the date and tags for content.  
**script.html** Links to scripts. I am using feather for icons.

### static

This is where static files go for the site - css **/css**, images **/img**, js files **/js**.

#### CSS

In the /css directory is where I put all my custom styling.  
**styles.css** > this is my overall custom styling that applies to every page. Defines colors, fonts, etc.  
**resume.css** > this stylesheet applies only to my resume page. It is referenced in resume.html using {{ "css/resume.css" | absURL }}

## Git Notes

This project is setup to use github pages / actions to run hugo and generate the site.

### /.github

hugo.yml is the instructions given to github pages to use hugo to generate the site and host on github pages. It is a github actions file.  

You can define a specific config.toml file to use when generating the hugo site here. This can be used to define specific site parameters for github pages, like changing the site baseURL to the github pages site url: https://nickquerques.github.io/nquerques/

### pushing to github

***git add .***  
adds curent directory to the git  

***git commit -m "comment for this commit"***  
creates commit of current changes and add a note to remember what the commit was about  

***git push origin master***  
push the commit to the master branch. Uploads changes to github master. Because github pages is already set up, this will trigger the github actions to update the website.  

***git pull origin master***  
I always like to do this afterwords incase new files were generated with the github actions. If the files mismatch the changes will have be merged with local. do this before beginning editing the site again.