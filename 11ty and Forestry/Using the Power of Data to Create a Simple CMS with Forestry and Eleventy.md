---
title: Using the Power of Data to Create a Simple CMS with Forestry and Eleventy
date: 2022-01-29
tags: ccjs
updated: 2024-11-20
---

# Using the Power of Data to Create a Simple CMS with Forestry and Eleventy

**Talk Idea, described briefly:**

As my website got more complex (a problem strictly of my own doing), I searched for a way to make my life easier. How could I add new content without having to open my editor and get distracted with something else? Better yet, how can I use the power of JSON, front matter, and Nunjucks to guarantee new content goes exactly where it should with all the proper styling? Enter Forestry CMS.

Forestry is a git-based CMS and Eleventy is a static site generator, both fully customizable to do only what you tell it to do. With the right configurations, you can use both to create a fully-featured website without too much hassle.

## Eleventy

### Homepage

<https://www.11ty.dev/>

![[Pasted image 20210821083307.png]]

### Description

Eleventy was created to be a JavaScript alternative to Jekyll. It is [zero-config](https://www.11ty.dev/docs/glossary/#zero-config) by default, but has flexible (and seemingly infinite) configuration options. Eleventy **works with your project's existing directory structure**, uses **independent template engines**, and **works with multiple template languages**. You can pick one or use them all together in a single project.

- [HTML `*.html`](https://www.11ty.dev/docs/languages/html/)
- [Markdown `*.md`](https://www.11ty.dev/docs/languages/markdown/)
- [JavaScript `*.11ty.js`](https://www.11ty.dev/docs/languages/javascript/)
- [Liquid `*.liquid`](https://www.11ty.dev/docs/languages/liquid/)
- [Nunjucks `*.njk`](https://www.11ty.dev/docs/languages/nunjucks/)
- [Handlebars `*.hbs`](https://www.11ty.dev/docs/languages/handlebars/)
- [Mustache `*.mustache`](https://www.11ty.dev/docs/languages/mustache/)
- [EJS `*.ejs`](https://www.11ty.dev/docs/languages/ejs/)
- [Haml `*.haml`](https://www.11ty.dev/docs/languages/haml/)
- [Pug `*.pug`](https://www.11ty.dev/docs/languages/pug/)

### How to Use it

The getting started documentation [can be found here](https://www.11ty.dev/docs/getting-started/). It can be installed locally or globally with npm: `npm install @11ty/eleventy` and uses a single `.eleventy.js` configuration file to manage things like plugins, input and output directories, custom template filters, and more.

## Forestry

### Homepage

<https://forestry.io/>

![[Pasted image 20210821083341.png]]

### Description

Forestry.io is a Git-backed CMS (content management system) for websites built using static site generators. It is entirely powered by Git, which means any changes made in the Forestry interface get committed upon save. You have the ability to create multiple sites based on different branches of the same repo and, depending on the SSG you're using, can save new posts as drafts. Forestry is able to edit Markdown, JSON, YAML and TOML files, no matter your SSG of choice.

### How to Use it

You connect your site by logging into your Git provider and selecting your repository. Forestry will add a batch of config files to a `.forestry` folder in your project root, which handles all the data and settings of your CMS. They have a "remote admin" option too that allows you to sign in and edit your page at a URL of your choice on your domain, i.e. _<https://your.website/admin_>.

## How I Got Here

Eleventy was the first SSG I tried out and it immediately felt natural to how I wanted to build my website. As time went on, I learned more about the extensibility of 11ty, and with my site being a playground for anything new I wanted to try, I implemented a lot of new changes. My site is fairly straightforward, consisting of a homepage, posts (blog), work (projects), and an about section.

### The Power of Front Matter

Each project, depending on its size or importance, needed to populate on the /work/ page in a designated section. On top of that, the smaller projects needed to also have a post associated with them, while the larger projects needed their own dedicated page. I realized front matter in my Markdown files was the answer to making this all happen.

There's so much to unpack with _how much is possible_, so bear with me!

#### Example of the /work/ Page

![[Pasted image 20210821084311.png]]

#### Example of a Dedicated Project Page

![[Pasted image 20210821084354.png]]

You can add tags and other front matter to each page you create - no matter the template language in use - and 11ty will handle it. In your templates, you can create loops based on the front matter in a page to conditionally display content. My template language of choice is Nunjucks, so I created two loops on my /work/ page to display the larger and smaller projects. The location of your files doesn't matter for front matter; all my featured work live in the /work/ directory while my "Other Projects" live in my /posts/ directory. The front matter can still be manipulated all the same, but the final URL will be dependent on the directory structure you've chosen.

#### Featured Projects

This loop is for the top portion.

```twig
{# src/work/index.njk #}

<section class="work__focus">
    {% for work in collections['work'] | reverse %}
        <div class="container neumorphism">
            <div class="content">
                <h2>{{ work.data.title }}</h2>
                <p>{{ work.data.description }}</p>
            </div>
            <a href="{{ work.url }}">
                {% image "https://screenshot.troyv.dev/" + work.data.website | urlencode + "/opengraph", work.data.title, [600, 900, 1200], ['webp', 'jpg'], '(max-width: 700px) 100vw, 1200px', 'has-box-shadow' %}
            </a>
        </div>
    {% endfor %}
</section>
```

Eleventy uses the term "collections" for any pages generated in the build, so the loop is looking for any single item - denoted as _work_ in the loop - in the collections object with the tag of "work." Variables are used in the template to output the data. `{{ work.data.title }}` will populate with the item's `title` front matter, for example. As long as those values exist in the front matter then it'll display in the template. You'll also notice I'm using a shortcode called `{% image %}` - this is an 11ty plugin that accepts an image and automatically generates responsive sizes based on the arguments you pass to it. In this case, I'm passing it the image I specified in the retrieved page's front matter as `featuredImage`. This is how each item on the /work/ page has its own associated image.

A similar method is used in the slider section at the bottom, but instead it looks for pages with the tag "project."

```twig
{# src/work/index.njk #}

<div class="projects__misc">
    {% set imgWidths = [300, 600] %}
    {% set imgFormats = ['webp', 'png'] %}
    {% set imgSizes = '33vw' %}
    {% set imgClasses = 'caption-image' %}
    {% for project in collections['project'] | reverse %}
        <figure class="caption-overlay">
            <a class="caption-link" href="{{ project.url }}">
                {% if project.data.featuredImage %}
                    {% image project.data.featuredImage, project.data.title, imgWidths, imgFormats, imgSizes, imgClasses %}
                {% else %}
                    {% image "https://screenshot.troyv.dev/" + project.data.website | urlencode, project.data.title, imgWidths, imgFormats, imgSizes, imgClasses %}
                {% endif %}
                <div class="caption">
                    <figcaption>
                        <h3>{{ project.data.shortname }}</h3>
                    </figcaption>
                </div>
            </a>
        </figure>
    {% endfor %}
</div>
```

The loops are in place, but you still have to connect the two together. In the markdown file for the page/post, you have to specify the right tags and/or layout file to use. Below is a post with a tag of "project," which means it'll be caught in the latter loop and rendered in the "Other Projects" section of the /work/ page. The `shortname` and `featuredImage` will both be used as well in the final result.

```yaml
# src/posts/express-cats.md

---
title: I Made an Express App About my Cats
description: What better way to practice Express, Pug, and Tailwind than a cat app?
layout: post
date: 2021-04-25
tags: ['post', 'project']
shortname: Express With Cats
featuredImage: './src/assets/img/express-cats.png'

---
```

#### Adding Forestry to the Mix

Forestry has a dedicated section to creating Front matter. Seen here are three different front matter templates: blog posts, work, and miscellaneous pages.

![[Pasted image 20210821091413.png]]

Diving into the blog post template, you can see all the fields available. These can be added, removed, conditionally shown in the editor, and can take the form of text, boolean, image uploads, date pickers, and more.

![[Pasted image 20210821091608.png]]

##### How to Use the Template

Each template can be assigned to a directory in the Forestry settings. You can see that I have a section in the sidebar for all my Posts, and here's how Forestry knows to find it.

![[Pasted image 20210821091916.png]]

Now, anytime you go to edit or create a post, the front matter template appears on the side, where you can input your data as needed.

![[Pasted image 20210821092147.png]]

```ad-note

FYI, if you configure Eleventy to use Nunjucks as your Markdown engine, then you can use the `{% image %}` shortcode in your posts too!

```

### This is Great, but What About Non-front Matter?

Also possible! As mentioned before, "Forestry is able to edit Markdown, JSON, YAML and TOML files, no matter your SSG of choice."

#### My Resume

I created a dedicated resume page (found in a tutorial) in my about section because I got tired of trying to create and update it in Word, or even Figma. The template is a nunjucks file - `resume.njk` - and gets all of its data from a JSON file in the /about/ directory - `resume.11tydata.json`. The `.11tydata` is a way to tell 11ty it's a data file and which specific template it belongs to. You can find the [entire layout file here](https://github.com/troyvassalotti/personal-site/blob/main/src/about/resume.njk), but I will only highlight a couple sections for the sake of this talk.

##### Example of My Resume

![[Pasted image 20210821094225.png]]

In the template, I have a loop going to show all my work experience. Since I have a data file created, I don't need to specify everything in a granular way; 11ty knows what I'm referring to when I say "experience" or "job.dates."

```twig
{# src/about/resume.njk #}

{%- for job in experience | reverse %}
    <li class="h-event p-experience {% if job.hideonprint %} hideOnPrint {% endif %}">
        <article>
            <div class="entrylist__header">
                <h3 class="p-name">{{ job.job }}</h3>
                <p class="dates">
                    <time datetime="{{ job.dates.start }}">{{ job.dates.start | mmyyyy }}</time>
                    - {% if job.dates.end %}
                        <time
                        datetime="{{ job.dates.end }}">{{ job.dates.end | mmyyyy }}</time>{% else %}
                        <span>Present</span>{% endif %}
                </p>
                <p class="organization p-org h-card">
                    <a href="{{ job.employer.website }}" class="p-name u-url"
                       rel="noreferrer nofollow">{{ job.employer.name }}</a> <span
                            aria-hidden="true">·</span> <span
                            class="p-location">{{ job.employer.city }}</span>
                </p>
            </div>
            <div class="entry__content p-summary">
                <p>{{ job.responsibilities }}</p>
            </div>
        </article>
    </li>
{% endfor -%}
```

What this does is looks for each entry in "experience" in my JSON file and returns all the data for the template. There is a key for `hideonprint` that will determine whether or not the job should show when you try to print my resume. (Yes, it prints well too!)

```json
{
  ...
  ...,
  "experience": [
    ...,
    ...,
    {
      "job": "Intern",
      "employer": {
        "name": "Terra's Kitchen",
        "city": "Baltimore, MD",
        "website": "https://thedailyrecord.com/2019/09/25/baltimore-based-terras-kitchen-files-for-bankruptcy/"
      },
      "dates": {
        "start": "2017-05-30",
        "end": "2017-09-01"
      },
      "responsibilities": "I was responsible for cataloging our inventory of content and creative works to help the team quickly find assets. Additionally, I researched relevant topics for writing and publishing blogs.",
      "hideonprint": true
    },
    {
      "job": "Marketing Production Coordinator",
      "employer": {
        "name": "Terra's Kitchen",
        "city": "Baltimore, MD",
        "website": "https://thedailyrecord.com/2019/09/25/baltimore-based-terras-kitchen-files-for-bankruptcy/"
      },
      "dates": {
        "start": "2017-09-02",
        "end": "2019-08-28"
      },
      "responsibilities": "I managed the email channel to drive conversions and engagement. As such, I designed and built custom mobile-responsive promotional and transactional templates, implemented automated workflows, list management, and improved our HubSpot deliverability score from a 1 to a 67 (avg. score = 50), greatly improving effectiveness of the channel. I also headed the SEO, which included maintaining our keyword database and Moz Pro reporting, identifying opportunities for dietary-specific landing pages that greatly increased organic performance."
    },
    ...,
    ...,
  ],
  ...
  ...
}
```

#### Adding Forestry to the Mix

I didn't want to have to open my text editor every time I wanted to update my resume; especially if it involved needing to adjust the page formatting as a Word document. By connecting the data file to Forestry, I'm able to make updates in the comfort of a CMS without the hassle of a word processor.

![[Pasted image 20210821094831.png]]

### Summary

Eleventy is a great static site generator with unlimited customization possibilities. It connects well to Forestry CMS for a nicer editing experience on your personal (or not) website.
