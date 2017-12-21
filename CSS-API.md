# Component CSS API

## Public and private

According to [component responsibility](CSS-RESPONSIBILITY.md), component API should be divided into:

* Public API:
    * External layout
    * Look
* Private API:
    * Internal layout = external layout of children

## Elements and modifiers

Components should provide a way to set up CSS for their root element, internal elements and possible states (modifiers). States or modifiers are variant CSS definitions used conditionally inside a component.

## Namespacing

The simplest solution would be to use semantic CSS classes, with proper namespacing ([BEM](https://en.bem.info/methodology/quick-start/) is just one possible convention). With CamelCased component names in React applications a simpler variant would be more useful:
* `ComponentName-elementName_stateName`
* `ComponentName_stateName`

with second and third part optional.

Element-name and state-name should be named according to their functions, not to their visual presence (e.g. `MyInput-reset` instead of `MyInput-button_red`). Why functional names for CSS classes? Because:
* it's not a job of a component to define its look
* it decouples `HTML` template structure from `CSS API` - both can be modified separatelly without affecting each other

Namespacing doesn't really solve problems absolutly, because those are still global CSS clasess, so:
- there is still possibility to use the same name for different component
- they still can be overwritten globally, even in unrelated code
- they may be used outside of its context
- they don't require to declare CSS dependency explicitly

There are other solutions that do better job here, like e.g. [CSS modules](https://github.com/css-modules/css-modules), [styled components](https://github.com/styled-components/styled-components) (or its variants) or [Shadow DOM](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Shadow_DOM) but for the purpose of this research let's use "vanilla" namespacing solution, because of its flexibility.

## Selectors

Any element inside any component may be targeted by the parent using a selector which is combined of a few dimensions:
* its component instance CSS class (optional)
* its component type CSS class
* its element/state CSS class

For example when setting the external layout of a `Component`:
```css
.Parent {}
.Parent .componentInstance1.Component {}
```

For example when customising the internal layout of a `Component`:
```css
.Parent .componentInstance1.Component .childElement1 {}
.Parent .componentInstance1.Component .childElement2 {}
```

Looking from the application level, e.g. when setting the look of a `Component`:
```css
.App.appTheme1 .Component .childElement1 {}
```

Setting the look of a `Parent`:
```css
.App.appTheme1 .Parent .componentInstance1.Component .childState1 {}
```

* ([see example](CSS-RESPONSIBILITY.md#components-are-not-responsible-for-their-look))


## Semantic versioning

TBD..
