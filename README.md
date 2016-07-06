# CSS Modular Pattern
An in-depth look at thinking about CSS in modular way.

>**_Preface_**  
>This is a port-over into git of a document I wrote to help front-end team members align their CSS coding approach.  It evolved from my experience with [SMACSS](https://smacss.com/ "A flexible guide to developing sites small and large.") and working with enterprise sized web sites. It’s re-write as a [condensed style guide here](https://github.com/nathanielkess/CSS-Style-Guide "A component base CSS style guide taking ques from OOCS and SMACSS.").


##Overview
This CSS pattern is an organization technique optimized for large-scale websites. It is based off [SMACSS](https://smacss.com/) and is meant to serve as an approach to authoring CSS and HTML. The goal of this outline is to sync up coding styles for project team members to ensure CSS/html code is predictable efficient and scalable. 

Read the [introduction of SMACSS](https://smacss.com/book/) to get a base understanding for the rest of this document.


###High level organization
At a high level, styles are broken out into three sections in a given CSS document: **base**, **layout** and **module**.

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/CodeScreenshot.jpg "CSS Organized - High level")

**Base:** Base styles are defaults directly applied to html selectors. This section sets any base styles that given elements should default to. For example, the default brand appearance for `<a>` tags. Base rules are typically applied directly to HTML tags (as opposed to IDs or classes).

**Layout** This space is reserved for major layout pieces that house the small bits (or modules) of the site. Things like the site header, footer and sidebar. These sections are traditionally styled using IDs so most of the selectors in this portion are targeted by a single ID.

**Module:** These are the reusable bits. Buttons, call to actions, carousels, lists etc... When a module is defined well it can be thought of it as a new item in your “toolbox”. You can drop a module anywhere on any pages and it should conform to fit within its parent with minimal to no adjustments in the CSS. Necessary adjustments per instance happen on the parent level and should not take much to override.

##Base
Base styles are applied directly to HTML attributes. They assume how an element should look in all occurrences site-wide. For websites that follow a brand (all websites, right?), this sections is perfect for setting global brand specific defaults; i.e. font faces, hyperlink colours etc.

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/baseRules.jpg "Base Rules")

The Base section is generally short and should be carefully constructed. Setting default styles on too many elements in this section (much like reset files do) will cause redundant overrides later on. It also leaves a wide gab for inconsistency. Consider the following Base styles from a commonly used [reset style sheet](https://github.com/murtaugh/HTML5-Reset/blob/master/assets/css/reset.css "Example of a reset style sheet"):

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/resetCSS.jpg "Reset Styles")

It’s common practice on reset files to blow out the browsers default styles on things like HTML lists. On CMS websites littered with WYSIWUG controls or copy resource files; it’s up to the interface developer to re-implement bullet lists styles anywhere that free text can exist (basically anywhere on the site). This is where inconsistencies and code duplication creep in. Even worse, you end up dancing around over-qualified CSS selectors as you go deeper into the site.

To explain further - lets assume that the reset CSS above is applied to the following website. Padding, margin and list-style-types are removed site-wide

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/SiteDefaultcopy.jpg "Website example")

Perhaps, later, on that website there’s some text in the “main content” section that includes bullets lists. To support this, you would have to re-apply (or override) the removed styles from the previous reset code block.

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/SiteWithBullets.jpg "Website example with bullets")
![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/mainContentResetOverride.jpg "Reset Override")

Then you might have a component, like a carousel, in the main content that uses a `<ul>` as it’s markup. This now involves overriding that already overridden styles!

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/SiteWithBulletsAndCarousel.jpg "Page with bullets and carousel")
![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/carouselOverride.jpg "Carousel override")

Notice how the selectors get longer and longer. This get worse the further in you go. Imagin you need to use that same “carousel” outside of `#mainContent`. You would need to duplicate the `<ul>` and `<li>` rules inside “.carousel” with `#mainContent` *unspecified*

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/carouselSideBar.jpg "Carousel in sidebar")
![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/carouselUnspecified.jpg "Carousel unspecified")

Avoid reset files, instead, carefully craft your default styles and consider how elements will need to be re-used across the website.
Please read the [Base section of SMACSS](https://smacss.com/book/type-base) for further details..

##Layout
The components of a page (or site) can be broken out into two categories: **Minor components** and **major components**. The minor components (or modules) are the small bits of the site – CTAs, buttons, carousels, accordions, sub navigations etc…

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/minorComponents.jpg "Page minor components")

The major components are the big “scaffolding” pieces that hold all the minor components in place – header, footer, sidebar, main content etc…

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/majorComponents.jpg "Page major components")

The major components _are_ the **layout** pieces and should sit in the **layout** section of the CSS document. Following this guideline is a simple way boost the predictability of CSS code. Consider the following task: You need to widen the sidebar to 300px to make room for advertisements.

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/addBanner.jpg "Add a banner")

It’s a simple ask and without following any organization pattern, it generally involves starting at the top of a CSS document and scrolling down until you find the `#sidebar` rule to make the update. You’ll have to repeat the same process for the `#mainContent` section since it will need to be reduced to compensate for the new sidebar width. Again, start at the top and work down until you find the `#mainContent` rule for the update. However, before you can make that update you’ll need to know the width of the parent container (`#siteContainer`) so you can do the math to figure out the new `#mainContent` width for everything to fit nicely. …Repeat the search for #siteContent.

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/cssUnorganized.jpg "Unorganized CSS")

Each time you search for a line of CSS you’re hunting through 100% of the other CSS rules. Note how “major components” of a page are greatly affected by one another. By keeping “major component” rules organized together in the **Layout** section of the CSS document; yourself and future developers can quickly filter out a rough 60% of the CSS rules and focus in on the just the layout pieces. This is a huge time saver.

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/cssOrganized.png "Organized CSS")

A final thought about the Layout section is the use of ID’s for CSS styling. Layout elements of a site are not mean to be reusable. Like a template, the markup for layout pieces are consistent from page to page and should be identified and styled with IDs. This pattern gives developers a great deal of insight about the CSS by just looking at the markup. A developer can assume that all the HTML markup with IDs applied are the big layout pieces (or some kind of JavaScript hook). And all the markup with classes applied are the reusable bits called "**modules**" explained in the next section.

Be sure to read the [Layout section of SMACSS](https://smacss.com/book/type-layout) for more details.

##Module

Modules or “minor components” are the bits that make up the content of the site. Carousels, twitter feeds, call-outs, sub navigations tabbed boxes etc. are examples of modules. Modules site within layout components or inside other modules. Modules are designed to exist as standalone components and, when built correctly, are reusable and easy to extend. When a module is created it should be built relative the entire website, not a particular page or section. It’s important to keep this last point in mind while developing; it will ensure modules are built for reusability.

To build a module, start by finding the **suitable lowest level parent element of a component**. Use that as the root of the module and build inwards. (Contrary to SMACSS, try to leverage the semantics of HTML tags as style hooks for the guts of a module. More on this later.) Consider the following: the site below has a grid of vehicles with titles, descriptions and a call to action buttons.

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/gridOfCars.jpg "Page with modules")

First find the **suitable lowest level parent element** to work from as the module: In this example you can identify a basic list (grid) of vehicles so perhaps we can use an unordered list as our markup and use the `<ul>` with a class of `.vehicleHighlights` as the lowest level parent element for the module. Like so:

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/ulRoot.png "Module: vehicleHighlights")

However, you should consider that one of the “**vehicleHighlight**” components might need to be reused elsewhere, like inside a carousel or the sidebar.

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/sideBarhightlight.jpg "CTA in Sidebar")

With that said, the lowest suitable parent should be at the root of a single component (the `<li>`), not multiple (the `<ul>`). This way the reusable bit becomes a stand-alone component (or module) agnostic of its parent element.

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/liRoot.png "Proper root level module in list")
![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/vehicleInSideBarCode.png "Proper root level module in sidebar")

Once you identify the suitable lowest level parent as your module’s root, the rest is very basic CSS and markup coding. However, in contradiction with SMACSS, try to leverage HTML tags as style hooks for the guts of a module. Generally there’s no need to dress up any of the child elements with classes, you can instead leverage the semantics of HTML 5 to apply styles. By following this approach other developers can make clear assumptions about the CSS by simply looking at the HTML markup. In the HTML markup below you can assume that each CSS class epresents a standalone module.

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/moduleNodes.png "CSS Module Nodes")

The following CSS shows the default styles for each of these standalone modules. Note how each module is “chunked” off as its own
self-contained node. Each “node” represents a single module.

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/cssModuleNodes.png "CSS Module Nodes")

Each module is clearly separated by a space and a single indent. This “**node**” structure applies to the "Layout" section too. The end result is a CSS document that is highly scannable and easy to navigate. With these patterns in place, developers can examine a CSS document in an organized hierarchical way, quickly filtering out big groups of CSS as they narrow down from **Section > Node > Line**. As opposed to the traditional top to bottom / line-by-line.

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/hierachy.png "CSS Organized for scanning")


###3 Guidelines for creating modules

**1. Always use a CSS class to define a module**  
Although a module may be used once at the time of creation, it can be used again on other pages down the road or even multiple times on the same page. For this reason it’s important to always use a class to style and identify a module. IDs should not be used to style a module because they are only allowed once per page. IDs should instead be reserved for Layout components, JavaScript hooks or instance-specific overrides.

**2. A module’s style is defined with one selector and not qualified with a tag**  
A powerful feature of the modular pattern is that **modules are reusable**. On order to maintain this feature each module’s default style should be written with the “**lowest selector specificity**”.

High selector specificity (poor):
![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/highSelector.jpg "poor selector specificity")

Low selector specificity (efficient):
![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/lowSelector.jpg "efficient selector specificity")

By using a single class name (without a qualifying tag) as the selector for a module you’re ensuring that, by default, the module that you’re creating is designed to be a stand-alone component. This allows you to drop the component anywhere on any page without regard of the root element or the parent structure.The CSS below does not follow by this guideline. It styles the `.vehicleHighlight` relative to the `#sidebar`.

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/highSelector.jpg "poor selector specificity")

It assumes “vehicleHighlight” will only ever exist in the sidebar. Later if you need to add “vehicleHightlight” inside a carousel you’ll have to duplicate the same code for the carousel.

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/carouselVehicle.jpg "poor selector specificity")

This code duplication is avoided by using single classes as the selector (**Low selector specificity**).

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/lowSelector.jpg "efficient selector specificity")

**3. A module’s default style does not have layout or position rules applied**

Another powerful feature of the modular pattern is: **modules are easy to extend**. To achieve this, a module's default style cannot contain any rules that affect it's size, position or surrounding elements. (margins, floats, widths, heights etc..) In the modular CSS pattern it's up to the parent container of a module to sets **size and position rules**. Read on to see an example that breaks this rule and how to correct it.

Earlier in this document we created a .vehicleHightlight module. let’s consider that same module used multiple times on the same page. Below you can see it is first used inside the main content of a page.

First instance of "VehicleHighlight" used.
![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/siteVehicleMainCopy.jpg "CTA in body copy")

Default styles for "vehicleHighlight" module.
![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/codeVehicleMainContent.jpg "CSS for CTA in body copy")

Next you need to use the same module elsewhere - inside a carousel and the sidebar.

"VehicleHighlight" modules used multiples times on a page.
![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/siteVehicleSidebarandCarousel.jpg "CTA in multiple spots")

"VehicleHighlight" poorly extended in "carousel" and "sideBar".
![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/codeOverrides.jpg "CSS for CTA in multiple spots")


Examine the highlighted CSS code. The yellow CSS in the default style node is where the problem starts. It breaks the “no layout or position” rule by setting a default **width**, **margin** and **float**. Because of this, every time this module gets reused those rules will need to be redundantly overridden (see the blue highlighted CSS).

This is avoided by removing the layout and position rules from the default module’s style (see code below). The only place where "vehicleHightlight" needs position rules applied is in the main content section. For this, the module gets **extended** by the parent node (see yellow highlighted code below).

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/codeOverrideNoPosition.jpg "relative position override")

This practice circles all the way back the layout section. Remember in an earlier example we were asked to increase the width of the sidebar to fit space for advertisements? If all of our modules were styled with **size and position rules** (fixed widths), than altering a major layout width (like the sidebar) would spell disaster. Each component on the page site-wide would require updates to their widths (and probably padding and margins etc…). The testing involved to correct this on a large website would be a daunting task.

![alt text](https://github.com/nathanielkess/CSS-Modular-Pattern/raw/master/assets/SiteFixedWidthModules.jpg "improper size settings at modular level")

Simply put, build each module as fluid components that conform to the available width set by their parent node. If position rules, like widths, needs to be set directly on a module, set the width from the parent container node in the CSS document.

This format fits in very well with responsive websites since majority of effort on a responsive sites revolve around altering the size of components as the view port changes. Because the default styles for each component start fluid in the Modular pattern, position and sizing rules are efficiently overridden as the media queries kick-in.

##Final thought
As outlined in [SMACSS](https://smacss.com/), it's important to remember that this pattern is not a library, framework or plugin. It's meant to help developers think about their approach and organization of code in consistent and systematic way. By following a pattern other developers can make safe assumptions about one another's code which boosts efficiency for development and maintenance.
