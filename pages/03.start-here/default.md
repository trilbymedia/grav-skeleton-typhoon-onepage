---
menu: Start Here
title: Start Here...
hero:
    content: Getting started with Typhoon Theme
---

# How to get Started with Typhoon

## Turn off "Start Here" Notice

To disable the getting started notice, simply edit your `user/config/themes/typhoon.yaml` in the `Notices` tab, and **remove** or **unpublish** this example notice.

## Configuration Instructions

To find out how to configure Typhoon Theme, check out the [Typhoon Theme Documenation](https://getgrav.org/premium/typhoon/docs?target=_blank) on the GetGrav.org site.

## Theme Modification

### Create a Custom Theme from Typhoon

It is strongly advised to create a new theme based on Typhoon if you want to make any modifications to the Twig templates, CSS etc.  Typhoon makes a fantastic base theme to build your custom theme on top of because it has already been built responsively, and with a powerful navigation system.  These are the most complex and time-consuming aspects of building any theme.

To create your new custom theme, simply install the `devtools` plugin for Grav.  Then from the CLI run the command:

```shell script
bin/plugin devtools newtheme
```

This will then ask you for some information about your new theme.  When it asks you how to create the new theme select **Copy**, then choose **Typhoon** as the theme to copy.  Of course this means you must have Typhoon already installed in your Grav installation.  This will then create a copy of Typhoon but with your new theme name.  From this point forward, make all your changes in the new theme.

### Modify the CSS

While Typhoon is highly customizable, you will undoubtedly want to make your own tweaks and modifications to the design of the theme. Some of this is done via CSS which is powered by the [TailwindCSS](https://tailwindcss.com/) framework. You will need to familiarize yourself with this framework, but have no fear, it's very well documented and has an active and supportive community.

#### Installing NPM to Compile CSS

99% of everything you need to make modifications is actually already available in Tailwind's utility classes, and you can apply these directly in your Twig templates, or even directly in your content using HTML or shortcodes.  

Depending on your platform, installing NPM is different. So please follow the official [NPM Installation Instructions](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm?target=_blank) to accomplish this. After you have NPM installed you will first need to install the required packages, you can do this by just typing `npm install` in the root of the typhoon theme:

! Replace `typhoon` with your custom theme name if you have already created a custom theme from Typhoon.

 ```shell script
cd user/themes/typhoon
npm install
```

In case you ever want to upgrade the packages (such as to use the latest version of TailwindCSS) run this:

```shell script
npm update
```

#### Developing Custom CSS

There are times when you need to step beyond the TailwindCSS-provided utility classes and use your own custom CSS.  This is most likely to occur when you have no ability to change the output and include the required classes.  For these situations, or when you want to make modifications to the core `tailwind.config.js` file or add your own custom css, you need to recompile the development `build/css/site.css` file.  The best way to do this is to run in a dynamic 'watch' mode that automatically recompiles when a change is detected:

```shell script
npm run dev
```

If you want to do a quick compile that does not watch, simply use:

```shell
npm run prod
```

The custom modifications should be put in the `css/custom/` folder.  If you create a new file you should reference that in the top level `css/site.css` file to ensure it gets picked up and compiled. Best practices for developing custom Tailwind CSS dictates that you should try to use the existing Tailwind classes via the `@apply` directive as much as possible to ensure global configuration trickles down to your custom CSS.

A good example of this approach can be found in the `css/custom/typography.css` file, and here's a short extract from it:

```sass
body {
  &.debug-screens:before {
    @apply left-inherit right-0;
  }

  @apply text-gray-700;
}

.site-logo {
  img, svg {
    @apply h-full;
  }
}
```

#### Building Tailwind for Production

Prior to JIT support in Tailwind CSS it was essential to compile your CSS for production before using.  This ensured that only the **required** classes were included in your production CSS.  JIT however has changed how we work with Tailwind.  With JIT enabled (the default for Typhoon), the `site.css` file is always **purged** and is already optimized to only include the required CSS files.  This means that you no longer have to build a specific production version of your CSS.  You can simply use the default `site.css`. 

However, you still should use `npm run dev` or `npm run prod` to ensure that the site.css always contains the Tailwind CSS classes you are using.

#### Troubleshooting CSS issues

The Tailwind CSS file which is compiled into `buld/css/site.css` file utilizes the `purgecss` package to only include CSS classes that are being used.  Vite is used to handle this process, and the configuration for where `purgecss` looks for classes is contained in the `tailwind.config.js` file.  Feel free to modify this file to include other locations if a class is not getting found and included in the production CSS.

By default, Typhoon's purge configuration looks like this:

```json
  purge: normalize([
    '../../config/**/*.yaml',
    '../../pages/**/*.md',
    './blueprints/**/*.yaml',
    './js/**/*.js',
    './templates/**/*.twig',
    './typhoon.yaml',
    './typhoon.php',
    './available-classes.md',
  ]), 
```

Feel free to edit or adjust this file to include paths and files that need to be inspected for possible Tailwind CSS classes. 

At the root of your Typhoon theme you can find an `available-classes.md` file that you can consult at any time. This file also allows keeping certain classes upon purge which is used for the build of the production css, even if they have never been used.

! Note: If you change the theme name, you should also change the `typhoon.yaml` and `typhoon.php` section in the purge configuration.

### Modify Twig Templates

Most modifications will take the form of editing the [Twig templates](https://twig.symfony.com/doc/3.x/templates.html) which controls the HTML but also is used for setting the Tailwind utility classes for CSS. These are organized in the `templates/` folder of the theme.

If you have already created a new custom theme based on Typhoon, then you can edit this as you need.  Also, you can override other twig files that come from plugins for example, simply copy the Twig file from the plugin (including any folder structure inside the plugin's templates folder) and copy into your theme's templates folder.  Then you can modify the twig as you need to get the desired result. 

A good example of this can be seen in the `templates/partials/pagination.html.twig` where the `pagination` plugin's existing twig partial, has been copied to Typhoon and modified to make use of Tailwind's CSS utility classes.  No custom CSS was required because all the modifications were made directly by modifying the Twig file.

## Using SVG Icons in Typhoon

If you want to use SVG icons in Typhoon, you can use the **Features** modular sub-page as an example to follow.

In the markdown we have a custom frontmatter variable defined called `features`:

```yaml
features:
    -
        title: 'Markdown Syntax'
        icon: tabler/pencil.svg
    -
        title: 'Twig Templating'
        icon: tabler/template.svg
    -
        title: 'Smart Caching'
        icon: tabler/bolt.svg
...
```

The relevant Twig code (`templates/modular/features.html.twig`) looks like this:

```twig
{% for feature in page.header.features %}
        <div class="flex duration-300 transform hover:scale-105">
        {% if page.header.variation == 'vertical' %}

            <div class="flex flex-col w-full rounded-md group hover:bg-gray-100 dark:hover:bg-gray-800">
                {% if feature.link %}
                <a class="group-hover:text-primary dark:group-hover:text-primary" href="{{ url(feature.link) }}">
                {% endif %}
                    <div class="text-gray-500 group-hover:text-primary rounded-md p-3 text-center">
                            {{ svg_icon(feature.icon, 'w-16 h-16 stroke-current stroke-[1.5] mx-auto')|raw }}
                    </div>
```

Here you can see that we're looping over the `page.header.features` and assigning each entry as a `feature` variable. Then at the bottom of the snippet you will see the `svg_icon()` Twig function called, that passes the name of the icon, and then takes a second argument of a string Tailwind classes to give the correct stroke, color etc.

If you want to target a specific icon directly, or use a different icon set, you can simply change the path, for example:

```twig
{{ svg_icon('brands/codeship.svg', 'w-16 h-16 stroke-current stroke-[1.5] mx-auto')|raw }}
```

Also feel free to play with the Tailwind classes to make the icons a different size, or color, etc.


## Removing Dark mode

If you only want to remove the "dark" mode from Typhoon theme, it requires a few manual steps.  These are best accomplished in a text editor.

edit the `tailwind.config.js` file in the root of the theme directory and comment out (or remove if you are confident!) the following entries:

```js
darkMode: 'class',
```

Make sure you save this file.  Now you have to remove all the `dark:` class definitions in the custom CSS.  This is a bit of a manual process, so use a good editor like VSCode, PHPStorm, etc.

Do a "find in files" for the `css/` folder and search all files for `dark:`, then one-by-one, remove any of these dark entries. If the CSS entry only has dark entries, then you can remove the whole block.  For example, you can remove this whole section:

```css
.notices a {
  @apply dark:text-gray-100;
  &:hover {
    @apply dark:text-white;
  }
}
```

but this CSS code:

```css
.has-submenu:hover {
    @apply bg-gray-100 dark:bg-gray-800 transition duration-300;
}
``` 

Should just be edited to remove the `dark:` entries only:

```css
.has-submenu:hover {
    @apply bg-gray-100 transition duration-300;
}
``` 

When there are no remaining `dark:` entries left, you should be able to compile the CSS with:

```
npm run prod
```

If you get no errors, then you have successfully removed dark mode from the CSS.

You can optionally remove any `dark:` classes from the `templates/` folder as these will no longer have any effect.  This is not going to cause any errors, but it will save a few bytes of HTML.
