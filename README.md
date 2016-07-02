# CSS-Modular-Pattern
An in-depth look at thinking about CSS in modular way.

>**_Prologue_**

>This is a port-over into git of a document I wrote to help front-end team members align their CSS coding approach.  It evolved from my experience with [SMACSS](https://smacss.com/ "A flexible guide to developing sites small and large.") and working with enterprise sized web sites. It’s re-write as a [condensed style guide here](https://github.com/nathanielkess/CSS-Style-Guide "A component base CSS style guide taking ques from OOCS and SMACSS.").

##CSS Modular Pattern
The following CSS pattern is an organization technique optimized for large-scale websites. It is based off [SMACSS](https://smacss.com/) and is meant to serve as an approach to authoring CSS and HTML. The goal of this outline is to sync up coding styles for project team members to ensure CSS/html code is predictable efficient and scalable. 

Read the introduction of [SMACSS here](https://smacss.com/book/) to get a base understanding for the rest of this document.


###High level organization
At a high level, styles are broken out into three sections in a given CSS document: **base**, **layout** and **module**.

**image placeholder**

**Base:** Base styles are defaults directly applied to html selectors. This section sets any base styles that given elements should default to. For example, the default brand appearance for `<a>` tags. Base rules are typically applied directly to HTML tags (as opposed to IDs or classes).

**Layout** This space is reserved for major layout pieces that house the small bits (or modules) of the site. Things like the site header, footer and sidebar. These sections are traditionally styled using IDs so most of the selectors in this portion are targeted by a single ID.

**Module:** These are the reusable bits. Buttons, call to actions, carousels, lists etc... When a module is defined well it can be thought of it as a new item in your “toolbox”. You can drop a module anywhere on any pages and it should conform to fit within its parent with minimal to no adjustments in the CSS. Necessary adjustments per instance happen on the parent level and should not take much to override.

##Base
Base styles are applied directly to HTML attributes. They assume how an element should look in all occurrences site-wide. For websites that follow a brand (all websites, right?), this sections is perfect for setting global brand specific defaults; i.e. font faces, hyperlink colours etc.

**image placeholdeer**

The Base section is generally short and should be carefully constructed. Setting default styles on too many elements in this section (much like reset files do) will cause redundant overrides later on. It also leaves a wide gab for inconsistency. Consider the following Base styles from a commonly used [reset style sheet](https://github.com/murtaugh/HTML5-Reset/blob/master/assets/css/reset.css "Example of a reset style sheet"):

**image placeholder**

It’s common practice on reset files to blow out the browsers default styles on things like HTML lists. On CMS websites littered with WYSIWUG controls or copy resource files; it’s up to the interface developer to re-implement bullet lists styles anywhere that free text can exist (basically anywhere on the site). This is where inconsistencies and code duplication creep in. Even worse, you end up dancing around over-qualified CSS selectors as you go deeper into the site.

To explain further - lets assume that the reset CSS above is applied to the following website. Padding, margin and list-style-types are removed site-wide


**Imaage placeholder**

Perhaps, later, on that website there’s some text in the “main content” section that includes bullets lists. To support this, you would have to re-apply (or override) the removed styles from the previous reset code block.

**image placeholder**

Then you might have a component, like a carousel, in the main content that uses a `<ul>` as it’s markup. This now involves overriding that already overridden styles!

**image**

Notice how the selectors get longer and longer. This get worse the further in you go. Image you need to use that same “carousel” outside of `#mainContent`. You would need to duplicate the `<ul>` and `<li>` rules inside “.carousel” with `#mainContent` *unspecified*

**image**

Avoid reset files, instead, carefully craft your default styles and consider how elements will need to be re-used across the website.
Please read the [Base section of SMACSS](https://smacss.com/book/type-base) for further details..

##Layout
The components of a page (or site) can be broken out into two categories: **Minor components** and **major components**. The minor components (or modules) are the small bits of the site – CTAs, buttons, carousels, accordions, sub navigations etc…

**Image**

The major components are the big “scaffolding” pieces that hold all the minor components in place – header, footer, sidebar, main content etc…

**Image**

The major components are the **layout** pieces and should sit in the **layout** section of the CSS document. Following this guideline is a simple way boost the predictability of CSS code. Consider the following task: You need to widen the sidebar to 300px to make room for advertisements.

**Image**

It’s a simple ask and without following any organization pattern, it generally involves starting at the top of a CSS document and scrolling down until you find the `#sidebar` rule to make the update. You’ll have to repeat the same process for the `#mainContent` section since it will need to be reduced to compensate for the new sidebar width. Again, start at the top and work down until you find the `#mainContent` rule for the update. However, before you can make that update you’ll need to know the width of the parent container (`#siteContainer`) so you can do the math to figure out the new `#mainContent` width for everything to fit nicely. …Repeat the search for #siteContent.

**Image**

Each time you search for a line of CSS you’re hunting through 100% of the other CSS rules. Note how “major components” of a page are greatly affected by one another. By keeping “major component” rules organized together in the **Layout** section of the CSS document; yourself and future developers can quickly filter out a rough 60% of the CSS rules and focus in on the just the layout pieces. This is a huge time saver.

**Image**

A final thought about the Layout section is the use of ID’s for CSS styling. Layout elements of a site are not mean to be reusable. Like a template, the markup for layout pieces are consistent from page to page and should be identified and styled with IDs. This pattern gives developers a great deal of insight about the CSS by just looking at the markup. A developer can assume that all the HTML markup with IDs applied are the big layout pieces (or some kind of JavaScript hook). And all the markup with classes applied are the reusable bits called "**modules**" explained in the next section.

Be sure to read the [Layout section of SMACSS](https://smacss.com/book/type-layout) for more details.

##Module

Modules or “minor components” are the bits that make up the content of the site. Carousels, twitter feeds, call-outs, sub navigations tabbed boxes etc. are examples of modules. Modules site within layout components or inside other modules. Modules are designed to exist as standalone components and, when built correctly, are reusable and easy to extend. When a module is created it should be built relative the entire website, not a particular page or section. It’s important to keep this last point in mind while developing; it will ensure modules are built for reusability.

To build a module, start by finding the **suitable lowest level parent element of a component**. Use that as the root of the module and build inwards. (In contradiction with SMACSS, try to leverage the semantics of HTML tags as style hooks for the guts of a module. More on this later.) Consider the following: the site below has a grid of vehicles with titles, descriptions and a call to action buttons.

**Image**

First find the **suitable lowest level parent element** to work from as the module: In this example you can identify a basic list (grid) of vehicles so perhaps we can use an unordered list as our markup and use the `<ul>` with a class of `.vehicleHighlights` as the lowest level parent element for the module. Like so:

**Image**

However, you should consider that one of the “**vehicleHighlight**” components might need to be reused elsewhere, like inside a carousel or the sidebar.

**Image**

With that said, the lowest suitable parent should be at the root of a single component (the `<li>`), not multiple (the `<ul>`). This way the reusable bit becomes a stand-alone component (or module) agnostic of its parent element.

**Image**

Once you identify the suitable lowest level parent as your module’s root, the rest is very basic CSS and markup coding. However, in contradiction with SMACSS, try to leverage HTML tags as style hooks for the guts of a module. Generally there’s no need to dress up any of the child elements with classes, you can instead leverage the semantics of HTML 5 to apply styles. By following this approach other developers can make clear assumptions about the CSS by simply looking at the HTML markup. In the HTML markup below you can assume that each CSS class epresents a standalone module.

**Image**

The following CSS shows the default styles for each of these standalone modules. Note how each module is “chunked” off as its own
self-contained node. Each “node” represents a single module.

**IMage**

Each module is clearly separated by a space and a single indent. This “**node**” structure applies to the "Layout" section too. The end result is a CSS document that is highly scannable and easy to navigate. With these patterns in place, developers can examine a CSS document in an organized hierarchical way, quickly filtering out big groups of CSS as they narrow down from **Section > Node > Line**. As opposed to the traditional top to bottom / line-by-line.

**Image**


###3 Guidelines for creating modules

