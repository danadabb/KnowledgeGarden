---
id: 1Fz07ioIPeUJxh90OClEp
title: Rendering
desc: ""
updated: 1643768298165
created: 1643170942151
---

## CSR, SSR, SSG, SPA

React as-is client side renders single page applications.

## CSR / SPA- client-side rendering, single page application

Most common way of using frameworks such as React, vue, ember etc.

Client-side rendering means that a website’s JavaScript is rendered in your browser, rather than on the website’s server.

- The server renders a blank page with a script tag pointing to the apps bundle
- blank page is sent to the clients browser which makes the relevant api calls and renders the contents of the page

strengths:

- renders fast on the server
- supports SPAs

weaknesses:

- no initial render - customer/client has to render so if the app is big it could be a problem

- empty body - no content to crawl for a search engine - ** SEO is the biggest weak point **

## SSR - server side rendering

When the user makes a requst to the webpage, the server prepares the html page and then sends it back.

## SSG - static-site generating

Page is also generated on the server however it is rendered at **build time.**

## When to use what?

- Is your content dynamic or relatively static?

- is SEO important?

For a documentation page or a website that doesn;t need to fetch dynamic data, but SEO is important, you could use SSG

For a blog, SEO is important but the content is dynamic. In this case, SSR would be a good candidate.

For a CRM (Customer Relationship Manager), SEO is not a concern and the content is dynamic therefore an SPA would be a good choice.

You could also use a mix of the above depending on specific pages or domains.
