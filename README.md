# Flawless Codestyle

> LESS Coding Guidelines

The following is a living document highlighting guidelines, conventions and best practises for using LESS as a style preprocessor.

Naming conventions rely on structured and meaningful classnames which allow developers to quickly and confidently write and update styles. Style encapsulation, isolation and performance are key tenets.

## Motivation

* Styling needs to be as encapsulated as possible (DOM limitations regarding CSS encapsulation aside).
* Styling should be component (or block) specific rather than page specific.
* Styling should be composable and modular.
* Preprocessors provide more than enough rope to hang yourself, these guidelines use only a small subset of the power of less and should be equally viable in other preprocessor environments or with regular old vanilla CSS implementations.
* This style guide should build on the superb work already existing in the CSS domain—most specifically the theories underpinning [suitcss](https://suitcss.github.io/), [bem](https://en.bem.info/) and [smacss](https://smacss.com/).
* The style guide should be mindful of the future—anything that deviates away from scope is likely to diminish in importance over time.
* CSS shouldn’t miss out on the prevalence of decent package managers—the strong preference is to use [npm](https://www.npmjs.com/).

## General Principles

* **Conventions should be adhered to**—the code base should be consistent and reliable, this is largely concerning naming conventions.
* **Use a strict subset of LESS**—variables, mixins and just one level of nesting (this is only to allow scoped variables).
* **Mixins should only be used to build utility classes**. Stamping out CSS using mixins should be avoided completely. Only ever use the function form of mixins, although parameterisation is all cool.
* **Nesting is to be avoided wherever possible**. Exceptions to this are declaring namespaces with scoped variables for components (due to processor limitations) and possibly applying pseudo and sibling classes. Nested media queries are also ok.
* **Use classes liberally**. Most elements will probably require classes, exceptions to this are long passages of repeated content (such as `<p>` in body copy). Using classes extensively helps to reduce unnecessarily long selectors.
* **Never style using IDs**.
* **Don’t prematurely optimise**—code should be clear and readable, wherever possible let tooling get you most of the way towards optimisation.
* **If in doubt examine the existing style and copy it**.

## Naming Conventions

Classes should be applied at the component level rather than at the page level. Naming conventions should aid developers when creating new components and quickly offer an explanation of their intended purpose to allow traceability, isolation of styling and confidence through code consistency.

### Components

`<ComponentName>[—descendentName|--modifierName].{is-stateName}`

```
.Profile-header
.Profile-avatar.is-animating
.Profile-mainName .Profile-mainName--default
```

#### `<Component Name>` _PascalCase_

These define the component root and are often analogous to the JS classes underpinning the component`

```
.Profile { /*...*/ }
```
```
<section class=“Profile”>
  ...
</section>
```

#### `[-descendentName]` _camelCase_

A component descendent is attached as a child of the component root and is responsible for direct presentation on behalf of the particular component.

```
<section class=“Profile”>
  <header class=“Profile-header”>
    ...
  </header>
  <div class=“Profile-body”>
    ...
  </div>
</section>
```

#### `[--modifierName]` _camelCase_

A component modifier class alters the base style set out by the component class. By appending them encapsulation is achieved (i.e. no indirect style leaks to other components) without the need for nesting or long selectors.

Modifiers need to be included in the HTML along with the class they modify.

```
.Profile-header { /*...*/ }
.Profile-header--featured { /*...*/ }
```
```
<section class=“Profile”>
  <header class=“Profile-header Profile-header--featured”>
    ...
  </header>
  <div class=“Profile-body”>
    ...
  </div>
</section>
```

#### `{.is-stateName}` _hyphenated_ _camelCase_

Components have state, that’s all good, these classes along for style modifications based on component state. State classes should always be scoped to the component to achieve encapsulation—**always apply state as an adjoining class**.

JS can and will add/remove classes, although template logic may also define them.

```
.Profile-avatar { /*...*/ }
.Profile-avatar.is-animating { /*...*/ }
```
```
<img class=“Profile-avatar is-animating” src=“...” alt=“...”>
```

### Javascript Control Classes

`js-<TargetName>`

These classes allow hooks for javascript to grab and **should not be styled**. They are not necessary everywhere but are often a useful tool.

The target name should follow the convention of the component (or class) it is being added to and use pascal case.

```
<button class=“Button Button--default js-Button></button>
```

### Utility Classes

`u-<utilityName>`

Utilities are low-level, structural classes and can applied directly to any element. They should be used for properties that are extensive and repetitious and exist to reduce code size and selector length (i.e. perf).

They are akin to some use examples of functional mixins, but without unnecessary code bloat.

Utility names should be camel cased and prefixed with `u-`.

```
.u-cleafix,
.u-cf { /*...*/ }
```

To increase modularity functional mixins can be used to construct utilities at the app level.

```
.m-makeColumn( @width: 200px ) {
  max-width: @width;
  width: 100%;
}

.u-col {
  .m-makeColumn( 400px );
}
```

### Variables

`@--<property>-<value>[--ComponentName]`

Variables should be very descriptive and structured. If they are used solely to define a component (and are scoped) then the `[--ComponentName]` can be dropped, which allow repurposing at the risk of losing specificity (only a problem with global variables).

The double hyphen identifier is to differentiate them from variables used as mixin parameters.

```
@--color-black: #000;
@--zIndex-2: 200;
@--fontSize-small: .85rem;
```

Variable interpolation is sometimes a necessity, but it’s all good.

```
@--sm-view: ~”only screen and ( max-width: 479px )”;

@media @--sm-view { /*...*/ }
```

### Mixins

`.m-<mixinName>[--modifierName]`

Mixins should be prefixed with `m-` and should probably also include `make` to be explicit that this mixin outputs styling.

```
.m-makeColumns( @width: 200px, @gutter: 20px ) { /*...*/ }
```

Polyfill mixins for browser-prefixed properties are good but rely on [autoprefixer](https://github.com/postcss/autoprefixer) rather than reinventing the wheel. Let tooling take the strain and relax.

Parametric variables should be declared hyphen-separated (no starting hyphen/s) to differentiate them from other variables.

## Formatting

`@TODO`

## Reference

Yep, a lot of this is copy-pasted (or barely adapted) from a number of other sources. Standing on the shoulders of giants.

[Suit CSS](https://suitcss.github.io/)
[Thanks @fat](https://medium.com/@fat/mediums-css-is-actually-pretty-fucking-good-b8e2a6c78b06)
[Thanks @necolas](https://github.com/necolas/idiomatic-css)
[bem](https://en.bem.info/)

