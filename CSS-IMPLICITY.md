## Explicit dependencies

According to best coding practices UI-components should:
- have no side-effects
- not be affected implicitly
- have only explicit dependencies

When component is not really isolated and may be affected by anything, it prevents scalability. While appliction grows and becomes more complex, implicit dependencies complicate testing and debugging, make it harder to understand and modify any part of code without introducing bugs elsewhere.

### JavaScript

The problem is fairly well solved within `JavaScript` due to modern component frameworks like React and Angular 2+, where components' state is isolated and may be set only through API (parameters/attributes). In contrast: it was fairly problematic in Angular 1 as `controller scope` was implicitly interited from the parent `controller scope`.

### CSS

But in `CSS` it is still a problem, due to inheritance:
https://developer.mozilla.org/en-US/docs/Web/CSS/inheritance
https://developer.mozilla.org/en-US/docs/Web/CSS/inherit

For `inherited properties` whenever CSS property is not set, or set to `inherit`, it will take the value from the parent. Components may be affected implicitly which is probably not even clear for a component developer, let alone for its users.

This **implicity** may be solved in a simple way, thanks to `all` property:
https://developer.mozilla.org/en-US/docs/Web/CSS/all

Setting `all: initial` in component root element reset all properties to it's initial values and prevent inheritance.

Small demo (uncomment `all: initial` and see the difference):
https://codepen.io/anon/pen/WXLLLg

Unfortunately no IE/Edge support for that:
https://caniuse.com/#feat=css-all
