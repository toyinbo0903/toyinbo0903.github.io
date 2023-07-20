---
title: "Great coffee with a conscience"
subtitle: Support sustainable farming while enjoying a cup
image: /img/home-jumbotron.jpg
blurb:
    heading: Why Kaldi?
    text: "Kaldi is the coffee store for everyone who believes that great coffee shouldn't just taste good, it should do good too. We source all of our beans directly from small scale sustainable farmers and make sure part of the profits are reinvested in their communities."
intro:
    heading: "What we offer"
    text: "Kaldi is the ultimate spot for coffee lovers who want to learn about their java’s origin and support the farmers that grew it. We take coffee production, roasting and brewing seriously and we’re glad to pass that knowledge to anyone."
products:
    - image: img/illustrations-coffee.svg
      text: "We sell green and roasted coffee beans that are sourced directly from independent farmers and farm cooperatives. We’re proud to offer a variety of coffee beans grown with great care for the environment and local communities. Check our post or contact us directly for current availability."
    - image: /img/illustrations-coffee-gear.svg
      text: "We offer a small, but carefully curated selection of brewing gear and tools for every taste and experience level. No matter if you roast your own beans or just bought your first french press, you’ll find a gadget to fall in love with in our shop."
values:
    heading: Our values
    text: Coffee is an amazing part of human culture but it has a dark side too – one of colonialism and mindless abuse of natural resources and human lives. We want to turn this around and return the coffee trade to the drink’s exhilarating, empowering and unifying nature.
---

# Hugo template for Decap CMS with Identity

This is a small business template built with [Hugo](https://gohugo.io) and [Decap CMS](https://github.com/decaporg/decap-cms), designed and developed by [Darin Dimitroff](https://twitter.com/deezel), [spacefarm.digital](https://www.spacefarm.digital).

## Getting started

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
