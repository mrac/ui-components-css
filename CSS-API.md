# Component CSS API

## Public and private

According to [component responsibility](CSS-RESPONSIBILITY.md) API should be divided into:

* Public API:
    * External layout
    * Look
* Private API:
    * Internal layout = external layout of children

## Elements and modifiers

Components should provide a way to set up CSS for their root element, internal elements and possible states (modifiers). States or modifiers are variant CSS definitions used conditionally inside a component, for its root element or just specific elements.

The simplest solution would be to use semantic CSS classes, with proper namespacing ([BEM](https://en.bem.info/methodology/quick-start/) is just one possible convention). With CamelCased component names in React applications a simpler variant would be more useful:
* `ComponentName-elementName_modifierName`
* `ComponentName_modifierName`

with second and third part optional.

Element-name and modifier-name should be named according to their functions, not to their visual presence (e.g. `MyInput-reset` instead of `MyInput-button_red`). Why functional names for CSS classes? Because:
* it's not a job of a component to define its look
* it decouples `HTML` template structure from `CSS API` - both can be modified separatelly without affecting each other

Namespacing doesn't really solve name conficts absolutly, because those are still global CSS clasess, so:
- there is still possibility to use the same name for different component
- they still can be overwritten globally, even in unrelated code
- they may be used outside of its context
- they don't require to declare CSS dependency explicitly

There are for sure better solutions like e.g. [CSS modules](https://github.com/css-modules/css-modules), [styled components](https://github.com/styled-components/styled-components) or [Shadow DOM](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Shadow_DOM) but for the purpose of this research let's use "vanilla" namespacing solution.
