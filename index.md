---
layout: post
title: Technical Writing Assessment
date: 2017-01-04T15:04:10.000Z
description: >-
  This assessment is to create a technical document in markdown and also share the rendered output. This technical documentation should explain how to calculate the Key Performance Indicator (KPI) Net Promotor Score using SQL
---

<html>
  <body>
    Here is one mermaid diagram:
    <pre class="mermaid">
            sequenceDiagram
    autonumber
    User->>Database: Request (SQL Query)
    Database->>Data: Fetch "customer_surveys"
    loop Calculate Counts
        Data->>Data: Calculate count of promoters, detractors and total responses
    end
    Note right of Data: Created response_counts subquery!
    Data-->>Database: Return result of subquery
    Database->>Data: Fetch from response_counts subquery
    loop Calculate NPS
        Data->>Data: Calculate NPS based on counts
    end
    Note right of Data: Calculated NPS!
    Data-->>Database: Return result of calculation
    Database-->>User: Response (NPS Result)
    </pre>

    <script type="module">
      import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.esm.min.mjs';
      mermaid.initialize({ startOnLoad: true });
    </script>
  </body>
</html>

sequenceDiagram
    autonumber
    User->>Database: Request (SQL Query)
    Database->>Data: Fetch "customer_surveys"
    loop Calculate Counts
        Data->>Data: Calculate count of promoters, detractors and total responses
    end
    Note right of Data: Created response_counts subquery!
    Data-->>Database: Return result of subquery
    Database->>Data: Fetch from response_counts subquery
    loop Calculate NPS
        Data->>Data: Calculate NPS based on counts
    end
    Note right of Data: Calculated NPS!
    Data-->>Database: Return result of calculation
    Database-->>User: Response (NPS Result)


<!-- The HTML -->
<div class="accordion">
  <div class="accordion-item">
    <button id="accordion-button-1" aria-expanded="false"><span class="accordion-title">Accordion Item 1</span></button>
    <div id="accordion-content-1" class="accordion-content">
      Content for Accordion Item 1...
    </div>
  </div>

  <div class="accordion-item">
    <button id="accordion-button-2" aria-expanded="false"><span class="accordion-title">Accordion Item 2</span></button>
    <div id="accordion-content-2" class="accordion-content">
      Content for Accordion Item 2...
    </div>
  </div>
</div>

<!-- The CSS -->
<style>
.accordion-content {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.2s ease-out;
}

.accordion .accordion-item button[aria-expanded='true'] + .accordion-content {
  max-height: 100px;
}
</style>

<!-- The JavaScript -->
<script>
const buttons = document.querySelectorAll(".accordion .accordion-item button");

buttons.forEach(button => {
  button.addEventListener("click", () => {
    const expanded = button.getAttribute("aria-expanded") === "true" || false;

    button.setAttribute("aria-expanded", !expanded);

    const content = button.nextElementSibling;
    content.style.maxHeight = !expanded ? content.scrollHeight + "px" : 0;
  });
});
</script>

# Table of Contents

- [Introduction](#introduction)
- [Chapter 1](#chapter-1)
- [Chapter 2](#chapter-2)
  - [Section 2.1](#section-21)
  - [Section 2.2](#section-22)
- [Conclusion](#conclusion)

## Introduction
Your introduction content...

## Chapter 1
Your chapter 1 content...

## Chapter 2
Your chapter 2 content...

### Section 2.1
Your section 2.1 content...

### Section 2.2
Your section 2.2 content...

## Conclusion
Your conclusion content...

<!-- HTML -->
<nav>
  <div class="hamburger-menu">
    <div></div>
    <div></div>
    <div></div>
  </div>
  <ul class="nav-links">
    <li><a href="#link1">Link 1</a></li>
    <li><a href="#link2">Link 2</a></li>
    <li><a href="#link3">Link 3</a></li>
  </ul>
</nav>

<!-- CSS -->
<style>
.hamburger-menu {
  display: none;
  flex-direction: column;
  justify-content: space-around;
  width: 2rem;
  height: 2rem;
  cursor: pointer;
}
.hamburger-menu div {
  width: 2rem;
  height: 0.25rem;
  background: currentColor;
}

.nav-links {
  display: flex;
  justify-content: space-around;
  list-style: none;
}

@media (max-width: 768px) {
  .nav-links {
    display: none;
  }
  .hamburger-menu {
    display: flex;
  }
}
</style>

<!-- JavaScript -->
<script>
const hamburgerMenu = document.querySelector('.hamburger-menu');
const navLinks = document.querySelector('.nav-links');

hamburgerMenu.addEventListener('click', () => {
  navLinks.style.display = navLinks.style.display === 'none' ? 'flex' : 'none';
});
</script>


# Hugo template for Decap CMS with Identity

This is a small business template built with [Hugo](https://gohugo.io) and [Decap CMS](https://github.com/decaporg/decap-cms), designed and developed by [Darin Dimitroff](https://twitter.com/deezel), [spacefarm.digital](https://www.spacefarm.digital).

## Get strted.

Use our deploy button to get your own copy of the repository. 

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/decaporg/one-click-hugo-cms&stack=cms)

This will setup everything needed for running the CMS:

* A new repository in your GitHub account with the code
* Full Continuous Deployment to Netlify's global CDN network
* Control users and access with Netlify Identity
* Manage content with Decap CMS

Once the initial build finishes, you can invite yourself as a user. Go to the Identity tab in your new site, click "Invite" and send yourself an invite.

Now you're all set, and you can start editing content!

## Local Development

Clone this repository, and run `yarn` or `npm install` from the new folder to install all required dependencies.

Then start the development server with `yarn start` or `npm start`.

## Testing

With the development server running, run the tests locally
with `yarn cypress:run` or `npm run cypress:run`.
Or use `yarn cypress:open` or `npm run cypress:open` to run interactively.

Cypress tests also run on deploy with the [Cypress Netlify integration](https://www.netlify.com/integrations/cypress/).

## Layouts

The template is based on small, content-agnostic partials that can be mixed and matched. The pre-built pages showcase just a few of the possible combinations. Refer to the `site/layouts/partials` folder for all available partials.

Use Hugo’s `dict` functionality to feed content into partials and avoid repeating yourself and creating discrepancies.

## CSS

The template uses a custom fork of Tachyons and PostCSS with cssnext and cssnano. To customize the template for your brand, refer to `src/css/imports/_variables.css` where most of the important global variables like colors and spacing are stored.

## SVG

All SVG icons stored in `site/static/img/icons` are automatically optimized with SVGO (gulp-svgmin) and concatenated into a single SVG sprite stored as a a partial called `svg.html`. Make sure you use consistent icons in terms of viewport and art direction for optimal results. Refer to an SVG via the `<use>` tag like so:

```
<svg width="16px" height="16px" class="db">
  <use xlink:href="#SVG-ID"></use>
</svg>
```
