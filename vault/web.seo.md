---
id: 3djj66lkeevulscgwm5lcjs
title: Search Engine Optimization (SEO)
desc: ""
updated: 1648110596024
created: 1648110596024
---

General guide for achieving good SEO for website:

- Build a sitemap
- Use SSR or SSGs - see [[web.frontend.designpatterns]]. SPAs usually don't use url paths and loading time is not ideal for a crawler.
- Use metadata such as `<title>` and `<meta name="description" content="A good description here">` on every page
- Ensure good performance e.g. response time
- Use alt for images
- Use meaningful status codes
- Use semantic HTML - the correct and most relevant tags

In November 2020, Google added three new metrics to their page experience signals called Core Web Vitals:

- Largest Contentful Paint (LCP) - The amount of time it takes to render the largest content element within the viewable area of the screen e.g. featured image
- First Input Delay (FID) - the time between the first users input and when the page responds (responsiveness)
- Cumulative Layout Shift (CLS) - visual stability of a page - checking for "janky-ness" where the screen shifts

### Largest Contentful Paint (LCP)

Common causes of poor LCP:

- **Slow server response times **- The first thing to look at is Time to First Byte (TFFB) and see if that can be improved.

- **Render blocking JS and CSS** - style sheets and javascript imports will block the HTML parser. Defer any non-critical JavaScript & CSS. CSS and JS files can be minified.

- Slow resource load times - optimize and compress images

- Client side rendering - If you're building a site that is mostly rendered on the client, you should be wary of the effect it can have on LCP if a large JS bundle is used.
