## Separate R Markdown navbar

When you create an R Markdown website the navigation bar is embedded into each HTML page.
This has some advantages (including simplicity) but a big disadvantage is that whenever you want to edit the navbar (say to add a new page) you have to re-knit every document.
For projects with lots of computation this becomes a big overhead.

This repository contains a slightly hacky workaround to avoid this problem.

You can see what the result looks like at https://lazappi.github.io/separate_rmarkdown_navbar/index.html.

## How it works

The navbar is usually defined in the sites `_site.yml` file but it is also possible to [provide HTML directly][html-navbar] using a `_navbar.html` file.
Here we abuse this approach to load content from a second file.

The `_navbar.html` file is a stub that contains a placeholder `<div>` and a short snippet of jQuery code.
We don't include the navbar content here because this file is embedded in the final HTML just like it would be if we used `_site.yml`.

The actual navbar content goes in a second file called `navbar-content.html`.
When the page is loaded the jQuery code reads `navbar-content.html` and inserts the content into the placeholder `<div>` in `_navbar.html`.
Becuause the content is only included when the page is viewed we can edit `navbar-content.html` as much as we like without having to re-knit anything.

## Serving locally

If you try to open one of your HTML file locally and the navbar is missing the jQuery code has probably been blocked by your browser
(you can check this in Chrome by right-clicking and selecting "Inspect").

To get around this you can set up a mini webserver using the **{servr}** package
(where `docs/` is the directory containing your rendered website).

```r
servr::httd("docs")
```

[html-navbar]: https://bookdown.org/yihui/rmarkdown/rmarkdown-site.html#html-navigation-bar "R Markdown HTML navigation bar"
