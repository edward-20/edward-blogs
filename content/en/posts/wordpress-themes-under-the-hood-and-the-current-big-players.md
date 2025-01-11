+++
date = '2025-01-08T13:32:45+11:00'
draft = false
title = 'Wordpress Themes Under the Hood and the Current Big Players'
tags = ['wordpress', 'php']
+++

As a volunteer developer I'm being asked to decide on the Wordpress theme to be
used for our online shopping website. I held the understanding that a theme is
literally just a collection of template files which are with the advent of the
block editor in Wordpress: HTML files with block markup. When a person visits a
post or page of a wordpress deployment the template hierarchy determines which
template file is used to render the content.

However a quick peruse through `r/wordpress` shows that some of the popular
themes in circulation are:
* Astra
* GeneratePress
* Kadence
* Divi
* Neve
* OceanWP

and that they hold the assumption that most users will use one of the popular
page builder.

Some questions that arise:
* if a theme is just a collection of template files, how come there's such a big
  ecosystem surrounding them? (the companies that make them, the integration with
  page builders)
* with Astra, why is it that I have to select starter templates after getting
  the theme, isn't that what the theme is for? isn't a theme supposed to give me
  a template?
* how come a theme impacts the editing experience? For example with Astra,
  there's now a banner in my dashboard prompting me to "Get Started with Starter
  Templates" where I didn't see that previously with other free themes I've used
  before

Obviously these questions are somewhat naive and all stem from my lack of
understanding of what a theme really is made of ...

## What's a theme?
```
assets/
  css/
    core-site-tile.css
  images/
    header-background.png
  js/
    navigation.js
inc/
  ClassName.php
  function-helpers.php
parts/
  footer.html
  header.html
patterns/
  example.php
styles/
  example.json
templates/
  404.html
  archive.html
  index.html (required)
  singular.html
README.txt
functions.php
screenshot.png
style.css (required)
theme.json
```

The only necessary files (labelled required) are for styling and a single
template file used as a fallback.

`parts` directory is for template parts, which make your templates more modular.

`patterns` is for block patterns.

`styles` is for variations on the theme's global settings and styles stored in
individual JSON files.

`assets` or `resources` or `public` for assets.

`inc` for custom PHP classes and additional functionality.

`functions.php` is loaded by Wordpress after the theme is initialised during the
page-loading process. 

`theme.json` for settings and styles of the site, usually configured via by GUI
as opposed to manual text editing.

### Functionality of the Theme
`functions.php` acts like a Wordpress plugin. Many theme developers means many
choices of what functionalities to provide. Here are the common denominators:

1. adding actions or filters (hook into certain stages of wordpress "lifecycle")
2. theme setup function; register theme-supported features with Wordpress. This
  is almost always executed on the `after_setup_theme` action hook, which is the
  first hook available after a theme’s functions.php file has been loaded. Setup
  functions are more common in classic themes. When using a block theme, the
  theme is often automatically opted into the features needed.
3. WordPress provides helper functions and specific action hooks for loading
  scripts and styles. This ensures that they are injected at the appropriate place
  in the document output. WordPress creates the appropriate HTML markup for you.
  It is not uncommon when building block themes to have no need of including
  additional scripts/styles. Some themes rely entirely on Global Settings and
  Styles for the front-end design.
4. Using other PHP files from `inc`

### Assets
If you are familiar with HTML, you might be accustomed to including CSS
stylesheets via the `<link rel=”stylesheet”/>` or `<style>` tags. The same might
be true for including JavaScript via the `<script>` tag. But you should never
manually hard code these HTML elements in your theme. 

WordPress has specific hooks for determining when to load scripts/styles and
functions for generating the markup. This ensures that WordPress, any active
plugins, and your theme all play nicely together.

### Global Settings
* Enable or disable features like drop caps, padding, margin, and line-height.
* Add a color palette, gradients, duotones, and shadows.
* Configure typographical features like font families, sizes, and more.
* Add CSS custom properties.
* Register custom templates and assign parts to template part areas.

## What's different about Astra?
In contrast to one of the free block themes I've chosen, it's quite
different.Right off the bat, I can tell it's not a block theme and so Wordpress'
documentation concerning the internals of a theme becomes a bit invalid since
it's in regards to block themes. We see this in the appearance link in the side
nav bar of the admin.

![astra-appearance](/astra-appearance.png)

I'm also given a banner in the admin prompting me to get started with a starter template.

![astra-banner](/astra-banner.png)

Also it has a dedicated link on the side nav bar of the wordpress admin.
![astra-banner](/astra-link.png)

Let's look at the directory structure:

```
.
├── 404.php
├── SECURITY.md
├── admin
│   ├── assets
│   ├── class-astra-admin-loader.php
│   ├── includes
│   └── jsconfig.json
├── archive.php
├── assets
│   ├── css
│   ├── fonts
│   ├── js
│   └── svg
├── changelog.txt
├── comments.php
├── footer.php
├── functions.php
├── header.php
├── inc
│   ├── addons
│   ├── admin-functions.php
│   ├── assets
│   ├── blog
│   ├── builder
│   ├── class-astra-after-setup-theme.php
│   ├── class-astra-dynamic-css.php
│   ├── class-astra-extended-base-dynamic-css.php
│   ├── class-astra-global-palette.php
│   ├── class-astra-loop.php
│   ├── class-astra-mobile-header.php
│   ├── compatibility
│   ├── core
│   ├── customizer
│   ├── dynamic-css
│   ├── extras.php
│   ├── google-fonts.php
│   ├── index.php
│   ├── lib
│   ├── markup-extras.php
│   ├── metabox
│   ├── modules
│   ├── schema
│   ├── template-parts.php
│   ├── template-tags.php
│   ├── theme-update
│   ├── w-org-version.php
│   └── widgets.php
├── index.php
├── languages
│   └── astra.pot
├── page.php
├── readme.txt
├── screenshot.jpg
├── search.php
├── searchform.php
├── sidebar.php
├── single.php
├── style.css
├── template-parts
│   ├── 404
│   ├── advanced-footer
│   ├── archive-banner.php
│   ├── blog
│   ├── content-404.php
│   ├── content-blog.php
│   ├── content-none.php
│   ├── content-page.php
│   ├── content-single.php
│   ├── content.php
│   ├── footer
│   ├── header
│   ├── index.php
│   ├── scroll-to-top.php
│   ├── single
│   ├── single-banner.php
│   └── special-banner.php
├── theme.json
├── toolset-config.json
└── wpml-config.xml
```

It's actually quite similar to the structure of the block theme except that
templates are now `php` files as opposed to `html` files. This literally seems
to be the only noteworthy difference. It has the necessary `style.css` file and
an `index.php` file which is a replacement for `index.html`. There are a few
differing things though:

### `SECURITY.MD`
This appears to be some documentation about how you can report bugs about Astra.
### `admin` directory
The admin directory seems interesting:
```
admin/
├── assets
│   ├── build
│   │   ├── dashboard-app-rtl.css
│   │   ├── dashboard-app.asset.php
│   │   ├── dashboard-app.css
│   │   ├── dashboard-app.css.map
│   │   ├── dashboard-app.js
│   │   └── dashboard-app.js.map
│   ├── components
│   │   ├── ProButton.js
│   │   ├── PromoCard.js
│   │   ├── icons
│   │   │   ├── Star.js
│   │   │   └── index.js
│   │   └── index.js
│   ├── hooks
│   │   ├── index.js
│   │   └── useDebounceEffect.js
│   ├── theme-builder
│   │   └── build
│   │       ├── index.asset.php
│   │       ├── index.css
│   │       ├── index.js
│   │       └── index.rtl.css
│   └── utils
│       └── helpers.js
├── class-astra-admin-loader.php
├── includes
│   ├── class-astra-admin-ajax.php
│   ├── class-astra-api-init.php
│   ├── class-astra-menu.php
│   └── class-astra-theme-builder-free.php
└── jsconfig.json 
```
The `includes` directory just has a class for each `php` file and I'm guessing
the names are clues to their purpose.

I'll take a guess that the `assets/build` is a SPA because it has bundled files and the `css` file is quite large.

`assets/components` are less obfuscated and is using JSX, and the `assets/hooks` are custom hooks wrapping react hooks.

`assets/theme-builder` has an `index.asset.php` file which 

```
<?php return array('dependencies' => array('react', 'react-dom', 'wp-i18n'), 'version' => '18bae9fc371e45eda5f8');
```
seems to be describing dependencies necessary for a React SPA. and the rest of the `assets/theme-builder` directory seems to be a built SPA.

After asking ChatGPT, it seems to be that all the React stuff is for user
interfaces to be used by admins of the Wordpress instance. And also some sort of
theme builder feature (I don't know Astra very well but I'm sure this will
become clear the more familiar one is with Astra). The
`class-astra-admin-loader.php` is the main php file using `includes` php files to handle backend logic for the admin interface.

### `toolset-config.json` and `wpml-config.xml`
Related to integration with popular WordPress plugins, Toolset Types and WPML
(WordPress Multilingual Plugin). The first being for creating custom post types,
fields and taxonomies. The second for translation into multiple languages.

## Conclusion and the economy of themes: how important they are to wordpress users
As a naive developer I suppose it'd be easy to say that a theme is a skin for a
wordpress website created through templates. But laymen use themes and expect
that they provide the means for more perhaps bootstrapping their website as
Astra does with AI and more functionalities. A theme can do what plugins do as
said by the wordpress documentation although this is discouraged. However with
the case of the world's most popular Wordpress theme I suppose there is some
expectation from its consumers that it will provide the functionality necessary
to support them and that paired with a page builder should achieve the bulk of
development features.

I have first hand experience with people who believe that functionality and
support comes from a theme, whose first move when starting with wordpress is
going to themeforest. So themes are important. Making themes can garner alot of
money.

[something interesting concerning "editable" themes](https://www.reddit.com/r/Wordpress/comments/1gulfex/i_completely_dont_understand_why_wp_devs_make/)