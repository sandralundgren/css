# Symbio CSS Styleguide

> Symbio's most reasonable approach to CSS and Sass

## Introduction

CSS is not a pretty language. While it is simple to learn and get started with, it soon becomes problematic at any reasonable scale. There isn’t much we can do to change how CSS works, but we can make changes to the way we author and structure it.

In working on large, long-running projects, with dozens of developers of differing specialities and abilities, it is important that we all work in a unified way in order to—among other things—

* Keep stylesheets maintainable;
* Keep code transparent, sane, and readable;
* Keep stylesheets scalable.

There are a variety of techniques we must employ in order to satisfy these goals, and this is a document of recommendations and approaches that will help us to do so.

## The Importance of a Styleguide

A coding styleguide (note, not a visual styleguide) is a valuable tool for teams who

* Build and maintain products for a reasonable length of time;
* Have developers of differing abilities and specialisms;
* Have a number of different developers working on a product at any given time;
* On-board new staff regularly;
* Have a number of codebases that developers dip in and out of.

Whilst styleguides are typically more suited to product teams—large codebases on long-lived and evolving projects, with multiple developers contributing over prolonged periods of time—all developers should strive for a degree of standardisation in their code.

A good styleguide, when well followed, will

* Set the standard for code quality across a codebase;
* Promote consistency across codebases;
* Give developers a feeling of familiarity across codebases;
* Increase productivity.

Styleguides should be learned, understood, and implemented at all times on a project which is governed by one, and any deviation must be fully justified.

## Syntax and Formatting

Having a standard way of writing (*literally* writing) CSS means that code will always look and feel familiar to all members of the team.

Further, code that looks clean *feels* clean. It is a much nicer environment to work in, and prompts other team members to maintain the standard of cleanliness that they found. Ugly code sets a bad precedent.

At a very high-level, we want

* Use soft tabs (2 spaces) for indentation
* Prefer dashes over camelCasing in class names. Underscores are OK if you're using BEM (see [OOCSS and BEM](#oocss-and-bem) below).
* Do not use ID selectors
* When using multiple selectors in a rule declaration, give each selector its own line.
* Put a space before the opening brace `{` in rule declarations
* In properties, put a space after, but not before, the `:` character.
* Put closing braces `}` of rule declarations on a new line
* Put blank lines between rule declarations
* 80 character wide columns
* Multi-line CSS
* Meaningful use of whitespace

But, as with anything, the specifics are somewhat irrelevant—consistency is key.

**Bad**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Good**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Multiple Files

With the meteoric rise of preprocessors of late, more often is the case that developers are splitting CSS across multiple files.

Even if not using a preprocessor, it is a good idea to split discrete chunks of code into their own files, which are concatenated during a build step.

If, for whatever reason, you are not working across multiple files, the next sections might require some bending to fit your setup.

### Table of Contents

A table of contents is a fairly substantial maintenance overhead, but the benefits it brings far outweigh any costs. It takes a diligent developer to keep a table of contents up to date, but it is well worth sticking with. An up-to-date table of contents provides a team with a single, canonical catalogue of what is in a CSS project, what it does, and in what order.

A simple table of contents will—in order, naturally—simply provide the name of the section and a brief summary of what it is and does, for example:

```css
/**
 * CONTENTS
 *
 * SETTINGS
 * Global...............Globally-available variables and config.
 *
 * TOOLS
 * Mixins...............Useful mixins.
 *
 * GENERIC
 * Normalize.css........A level playing field.
 * Box-sizing...........Better default `box-sizing`.
 *
 * BASE
 * Headings.............H1–H6 styles.
 *
 * OBJECTS
 * Wrappers.............Wrapping and constraining elements.
 *
 * COMPONENTS
 * Page-head............The main page header.
 * Page-foot............The main page footer.
 * Buttons..............Button elements.
 *
 * TRUMPS
 * Text.................Text helpers.
 */
```

Each item maps to a section and/or include.

Naturally, this section would be substantially larger on the majority of projects, but hopefully we can see how this section—in the master stylesheet—provides developers with a project-wide view of what is being used where, and why.

### 80 Characters Wide

Where possible, limit CSS files’ width to 80 characters. Reasons for this include

* The ability to have multiple files open side by side;
* Viewing CSS on sites like GitHub, or in terminal windows;
* Providing a comfortable line length for comments.

```css
/**
 * I am a long-form comment. I describe, in detail, the CSS that follows. I am
 * such a long comment that I easily break the 80 character limit, so I am
 * broken across several lines.
 */
```

### Titling

Begin every new major section of a CSS project with a title:

```css
/*------------------------------------*\
    #SECTION-TITLE
\*------------------------------------*/

.selector {}
```

The title of the section is prefixed with a hash (`#`) symbol to allow us to perform more targeted searches (e.g. `grep`, etc.): instead of searching for just `SECTION-TITLE` – which may yield many results—a more scoped search of `#SECTION-TITLE should return only the section in question.

Leave a carriage return between this title and the next line of code (be that a comment, some Sass, or some CSS).

If you are working on a project where each section is its own file, this title should appear at the top of each one. If you are working on a project with multiple sections per file, each title should be preceded by five (5) carriage returns. This extra whitespace coupled with a title makes new sections much easier to spot when scrolling through large files:

```css
/*------------------------------------*\
    #A-SECTION
\*------------------------------------*/

.selector {}





/*------------------------------------*\
    #ANOTHER-SECTION
\*------------------------------------*/

/**
 * Comment
 */

.another-selector {}
```

There will be unavoidable exceptions to this rule—such as URLs, or gradient syntax—which shouldn’t be worried about.

###Anatomy of a Ruleset

Before we discuss how we write out our rulesets, let’s first familiarise ourselves with the relevant terminology:

```css
[selector] {
  [property]: [value];
  [<--declaration--->]
}
```

For example:

```css
.foo, .foo--bar,
.baz {
  display: block;
  background-color: green;
  color: red;
}
```

Here you can see we have

* Related selectors on the same line; unrelated selectors on new lines;
* A space before our opening brace (`{`);
* Properties and values on the same line;
* A space after our property–value delimiting colon (`:`);
* Each declaration on its own new line;
* The opening brace (`{`) on the same line as our last selector;
* Our first declaration on a new line after our opening brace (`{`);
* Our closing brace (`}`) on its own new line;
* Each declaration indented by two (2) spaces;
* A trailing semi-colon (`;`) on our last declaration.

This format seems to be the largely universal standard (except for variations in number of spaces, with a lot of developers preferring two (2)).

As such, the following would be incorrect:

```css
.foo, .foo--bar, .baz
{
  display:block;
  background-color:green;
  color:red }
```

Problems here include

* Tabs instead of spaces;
* Unrelated selectors on the same line;
* The opening brace (`{`) on its own line;
* The closing brace (`}`) does not sit on its own line;
* The trailing (and, admittedly, optional) semi-colon (`;`) is missing;
* No spaces after colons (`:`).

### Indenting

As well as indenting individual declarations, indent entire related rulesets to signal their relation to one another, for example:

```css
.foo {}

    .foo__bar {}

        .foo__baz {}
```

By doing this, a developer can see at a glance that `.foo__baz {}` lives inside `.foo__bar {}` lives inside `.foo {}.

This quasi-replication of the DOM tells developers a lot about where classes are expected to be used without them having to refer to a snippet of HTML.

### Indenting Sass

Sass provides nesting functionality. That is to say, by writing this:

```scss
.foo {
    color: red;

    .bar {
        color: blue;
    }

}
```

…we will be left with this compiled CSS:

```css
.foo { color: red; }
.foo .bar { color: blue; }
```

When indenting Sass, we stick to the same two (2) spaces, and we also leave a blank line before and after the nested ruleset.

**N.B.** Nesting in Sass should be avoided wherever possible. But if you have to, do not nest selectors more than three levels deep!

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

### OOCSS and BEM

**OOCSS**, or “Object Oriented CSS”, is an approach for writing CSS that encourages you to think about your stylesheets as a collection of “objects”: reusuable, repeatable snippets that can be used independently throughout a website.

  * Nicole Sullivan's [OOCSS wiki](https://github.com/stubbornella/oocss/wiki)
  * Smashing Magazine's [Introduction to OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)

**BEM**, or “Block-Element-Modifier”, is a _naming convention_ for classes in HTML and CSS. It was originally developed by Yandex with large codebases and scalability in mind, and can serve as a solid set of guidelines for implementing OOCSS.

  * CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
  * Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

**Example**

```html
<article class="listing-card listing-card--featured">

  <h1 class="listing-card__title">Adorable 2BR in the sunny Mission</h1>

  <div class="listing-card__content">
    <p>Vestibulum id ligula porta felis euismod semper.</p>
  </div>

</article>
```

```css
.listing-card { }
.listing-card--featured { }
.listing-card__title { }
.listing-card__content { }
```

* `.listing-card` is the “block” and represents the higher-level component
* `.listing-card__title` is an “element” and represents a descendant of `.listing-card` that helps compose the block as a whole.
* `.listing-card--featured` is a “modifier” and represents a different state or variation on the `.listing-card` block.

## Commenting

The cognitive overhead of working with CSS is huge. With so much to be aware of, and so many project-specific nuances to remember, the worst situation most developers find themselves in is being the-person-who-didn’t-write-this-code. Remembering your own classes, rules, objects, and helpers is manageable *to an extent*, but anyone inheriting CSS barely stands a chance.

**CSS needs more comments.**

As CSS is something of a declarative language that doesn’t really leave much of a paper-trail, it is often hard to discern – from looking at the CSS alone –

* Whether some CSS relies on other code elsewhere;
* What effect changing some code will have elsewhere;
* Where else some CSS might be used;
* What styles something might inherit (intentionally or otherwise);
* What styles something might pass on (intentionally or otherwise);
* Where the author intended a piece of CSS to be used.

This doesn’t even take into account some of CSS’ many quirks—such as various sates of `overflow triggering block formatting context, or certain transform properties triggering hardware acceleration—that make it even more baffling to developers inheriting projects.

As a result of CSS not telling its own story very well, it is a language that really does benefit from being heavily commented.

As a rule, you should comment anything that isn’t immediately obvious from the code alone. That is to say, there is no need to tell someone that `color: red;` will make something red, but if you’re using `overflow: hidden;` to clear floats — as opposed to clipping an element’s overflow — this is probably something worth documenting.

### High-level

For large comments that document entire sections or components, we use a DocBlock-esque multi-line comment which adheres to our 80 column width.

```css
/**
 * The site’s main page-head can have two different states:
 *
 * 1) Regular page-head with no backgrounds or extra treatments; it just
 *    contains the logo and nav.
 * 2) A masthead that has a fluid-height (becoming fixed after a certain point)
 *    which has a large background image, and some supporting text.
 *
 * The regular page-head is incredibly simple, but the masthead version has some
 * slightly intermingled dependency with the wrapper that lives inside it.
 */
```

This level of detail should be the norm for all non-trivial code—descriptions of states, permutations, conditions, and treatments.

#### Object–Extension Pointers

When working across multiple partials, or in an OOCSS manner, you will often find that rulesets that can work in conjunction with each other are not always in the same file or location. For example, you may have a generic button object — which provides purely structural styles — which is to be extended in a component-level partial which will add cosmetics. We document this relationship across files with simple _object–extension pointers_. In the object file:

```css
/**
 * Extend `.btn {}` in _components.buttons.scss.
 */

.btn {}
```

And in your theme file:

```css
/**
 * These rules extend `.btn {}` in _objects.buttons.scss.
 */

.btn--positive {}

.btn--negative {}
```

This simple, low effort commenting can make a lot of difference to developers who are unaware of relationships across projects, or who are wanting to know how, why, and where other styles might be being inherited from.

### Low-level

Oftentimes we want to comment on specific declarations (i.e. lines) in a ruleset. To do this we use a kind of reverse footnote. Here is a more complex comment detailing the larger site headers mentioned above:

```css
/**
 * Large site headers act more like mastheads. They have a faux-fluid-height
 * which is controlled by the wrapping element inside it.
 *
 * 1. Mastheads will typically have dark backgrounds, so we need to make sure
 *    the contrast is okay. This value is subject to change as the background
 *    image changes.
 * 2. We need to delegate a lot of the masthead’s layout to its wrapper element
 *    rather than the masthead itself: it is to this wrapper that most things
 *    are positioned.
 * 3. The wrapper needs positioning context for us to lay our nav and masthead
 *    text in.
 * 4. Faux-fluid-height technique: simply create the illusion of fluid height by
 *    creating space via a percentage padding, and then position everything over
 *    the top of that. This percentage gives us a 16:9 ratio.
 * 5. When the viewport is at 758px wide, our 16:9 ratio means that the masthead
 *    is currently rendered at 480px high. Let’s…
 * 6. …seamlessly snip off the fluid feature at this height, and…
 * 7. …fix the height at 480px. This means that we should see no jumps in height
 *    as the masthead moves from fluid to fixed. This actual value takes into
 *    account the padding and the top border on the header itself.
 */

.page-head--masthead {
    margin-bottom: 0;
    background: url(/img/css/masthead.jpg) center center #2e2620;
    @include vendor(background-size, cover);
    color: $color-masthead; /* [1] */
    border-top-color: $color-masthead;
    border-bottom-width: 0;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1) inset;

    @include media-query(lap-and-up) {
        background-image: url(/img/css/masthead-medium.jpg);
    }

    @include media-query(desk) {
        background-image: url(/img/css/masthead-large.jpg);
    }

    > .wrapper { /* [2] */
        position: relative; /* [3] */
        padding-top: 56.25%; /* [4] */

        @media screen and (min-width: 758px) { /* [5] */
            padding-top: 0; /* [6] */
            height: $header-max-height - double($spacing-unit) - $header-border-width; /* [7] */
        }

    }

}
```

These types of comment allow us to keep all of our documentation in one place whilst referring to the parts of the ruleset to which they belong.

## Removing Comments

It should go without saying that no comments should make their way into production environments—all CSS should be minified, resulting in loss of comments, before being deployed.

## JavaScript Hooks

As a rule, it is unwise to bind your CSS and your JS onto the same class in your HTML. This is because doing so means you can’t have (or remove) one without (removing) the other. It is much cleaner, much more transparent, and much more maintainable to bind your JS onto specific classes.

I have known occasions before when trying to refactor some CSS has unwittingly removed JS functionality because the two were tied to each other—it was impossible to have one without the other.

Typically, these are classes that are prepended with `js-, for example:

```html
<input type="submit" class="btn  js-btn" value="Follow" />
```

This means that we can have an element elsewhere which can carry with style of `.btn {}`, but without the behaviour of `.js-btn.

### `data-* Attributes

A common practice is to use `data-*` attributes as JS hooks, but this is incorrect. `data-*` attributes, as per the spec, are used **to store custom data** private to the page or application (emphasis mine). `data-*` attributes are designed to store data, not be bound to.

### Taking It Further

As previously mentioned, these are very simple naming conventions, and ones that don’t do much more than denote three distinct groups of class.

We would encourage you to read up on and further look in to your naming convention in order to provide more functionality.

### Further Reading

* [MindBEMding – getting your head ’round BEM syntax](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

---

## Footnote 

### Roadmap 

Stuff that needs to be added:

- [ ] Selector Performance
- [ ] DRY (*Don’t Repeat Repeat Yourself*)
- [ ] Accessibility

### Current Maintainer

* [Johnie Hjelm](http://github.com/johnie) – [johnie.hjelm@symbio.com](mailto:johnie.hjelm@symbio.com)
