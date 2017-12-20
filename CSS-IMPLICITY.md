## Component isolation

According to best coding practices UI-components should:
- have no side-effects
- not be affected implicitly
- have only explicit dependencies

When component is not really isolated and may be affected by anything, it prevents scalability. While application grows and becomes more complex, implicit dependencies complicate testing and debugging, make it harder to understand and modify any part of code without introducing bugs elsewhere.

### JavaScript/HTML

The problem is fairly well solved within `JavaScript`/`HTML` due to modern component frameworks like React and Angular 2+, where components' state is isolated and may be set only through API (parameters/attributes). In contrast: it was fairly problematic in Angular 1 as `controller scope` was implicitly inherited from the parent `controller scope`.

### CSS inheritance - problem

But in `CSS` there is still a problem, due to inheritance:
* https://developer.mozilla.org/en-US/docs/Web/CSS/inheritance
* https://developer.mozilla.org/en-US/docs/Web/CSS/inherit

For `inherited properties` whenever CSS property is not set, or set to `inherit`, it will take the value from the parent. Components layout and look may be affected implicitly which is probably not even clearly predicted by component-library developers, let alone for its users.

### CSS inheritance - solution

This **implicity** may be solved in a simple way, thanks to `all` property:
* https://developer.mozilla.org/en-US/docs/Web/CSS/all

Setting `all: initial` in component root element reset all properties to its initial values and prevent CSS inheritance. Then the only way to set the component CSS is to target the component's selector in its parent/ancestor and explicitly set CSS. It's yet far from having the [CSS API](CSS-API.md) but very helpful already with isolation is place.

Small demo (uncomment `all: initial` and see the difference):
* https://codepen.io/anon/pen/WXLLLg

Unfortunately no IE/Edge support for that:
* https://caniuse.com/#feat=css-all

### CSS positioning - problem

Similarly, what may implicitly affect component layout is positioning. If component
has `position:absolute` elements but doesn't define a positioning context for them (e.g. set `position:relative` somewhere upper in hierarchy), the context will be implicitly defined by the environment where the component is placed in.

### CSS positioning - solution

Fortunately that case seems to be more obvious. A good practice would be to ALWAYS set `position:relative` for component's root element, as default.

Small demo below (uncomment `width: 200px` and see how positioning of some component's internals is based on the context of its parent. Then uncomment `position:relative` to fix that):
* https://codepen.io/mrac/pen/ypJegJ

This will set the positioning context for component's internals. Not only for 2 dimensions (`top`, `bottom`, `left`, `right`) but also 3D positioning, as `z-index` is a number that is interpreted as relative to its stacking context:
* https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context
