# CSS-Modular-Pattern
An in-depth look at thinking about CSS in modular way.

>**_Prologue_**

>This is a port-over into git of a document I wrote to help front-end team members align their CSS coding approach.  It evolved from my experience with [SMACSS](https://smacss.com/ "A flexible guide to developing sites small and large.") and working with enterprise sized web sites. It’s re-write as a [condensed style guide here](https://github.com/nathanielkess/CSS-Style-Guide "A component base CSS style guide taking ques from OOCS and SMACSS.").

##CSS Modular Pattern
The following CSS pattern is an organization technique optimized for large-scale websites. It is based off [SMACSS](https://smacss.com/) and is meant to serve as an approach to authoring CSS and HTML. The goal of this outline is to sync up coding styles for project team members to ensure CSS/html code is predictable efficient and scalable. 

Read the introduction of [SMACSS here](https://smacss.com/book/) to get a base understanding for the rest of this document.


###High level organization (following the SMACSS pattern)
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

Notice how the selectors get longer and longer. This get worse the further in you go. Image you need to use that same “carousel” outside of `#mainContent`. You would need to duplicate the `<ul>` and `<li>` rules inside “.carousel” with `#mainContent` unspecified.
