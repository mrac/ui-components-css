# Component responsibility

`CSS` is used to set components' **layout** and **look**.

* [Layout](CSS-RESPONSIBILITY.md#layout)
    * [External layout](CSS-RESPONSIBILITY.md#external-layout)
    * [Internal layout](CSS-RESPONSIBILITY.md#internal-layout)
    * [Setting vs customization](CSS-RESPONSIBILITY.md#setting-vs-customization)
        * [External-layout properties](CSS-RESPONSIBILITY.md#external-layout-properties)
        * [Position property](CSS-RESPONSIBILITY.md#position-property)
        * [Display property](CSS-RESPONSIBILITY.md#display-property)
        * [Fixed size](CSS-RESPONSIBILITY.md#fixed-size)
        * [Box sizing](CSS-RESPONSIBILITY.md#box-sizing)
        * [Internal-layout properties](CSS-RESPONSIBILITY.md#internal-layout-properties)
* [Look](CSS-RESPONSIBILITY.md#look)
    * [Components are not responsible for their look](CSS-RESPONSIBILITY.md#components-are-not-responsible-for-their-look)

## Layout

Layout is about setting the compoment's place in a web document. In old days before `CSS` has become mature technology, it was mainly accomplished by `HTML` markup (`width`, `height` attributes, excesive usage of `table` element, order of tags).

It answers the questions: *where?*, *how big?*, *how far from other components?*
We can see a web document as 3D space where components may be placed.

Layout is usually defined by the following CSS properties:
- `position`
- `display`
- `flex-*`, `align-*`, `order`, `justify-content`
- `float`, `clear`
- `left`, `right`, `top`, `bottom`
- `margin`, `margin-*`, `border`, `border-*`, `padding`, `padding-*`
- `width`, `height`, `max-width`, `max-height`, `min-width`, `min-height`
- `box-sizing`
- `z-index`
- `overflow`, `overflow-*`

There can be 2 types of layout CSS:
- external layout - how the component interracts with siblings and ancestors
- internal layout - the layout of the children within the component

### External layout

**External layout** defines how component is placed inside its parent. It is mainly associated with **root element** of the component.

It may be set entirely IN the parent component, for example:

```html
<MyForm>
    <MyInput class="my-input-1"></MyInput>
    <MyInput class="my-input-2"></MyInput>
</MyForm>
```

```css
.MyForm {
    display: flex;
    flex-direction: row-reverse;
    justify-content: center;
    align-items: flex-end;
    padding: 50px 20px;
}
```

Or may be set in the root element of the component BY the parent component, for example:

```css
.MyForm {
    display: block;
}
.MyForm .my-input-1 {
    display: inline;
    margin: 10px 50px;
    width: 100px;
}
.MyForm .my-input-2 {
    display: inline;
    margin: 10px 30px;
    width: 200px;
}
```

In both cases however it looks like the component itself should never be responsible for setting its own **external layout**. This responsibility should be left to its ancestors.

Nevertheless default values may be preset:

```css
.MyInput {
    display: block;
    width: 100%;
}
```

### Internal layout

**Internal layout** defines how component internal elements are placed within component's own space. Those CSS settings may be set up in the root element of the component and in the descendants. The descendants may also be custom components so in another words: it's about setting their external layout CSS.

It is the responsibility of the component to set up its own internal layout.

However, component's parent/ancestors may want to overwrite/customise it.

### Setting vs customization

Summarizing: components are responsible for setting its internal layout, but not external. In terms of API they have to provide a way to set the external layout, but it would be also very convenient if they provide a way to customise/overwrite their internal layout.

Those two cases however seem to be operations of different access:
- setting external layout (possibly only modifying CSS of the root element) - **"public"**, **safe**
- overwriting internal layout (modifying CSS of root element and/or internals) - may be marked **"private"**, possibly **unsafe**

It would be nice to decouple those 2 types of access. Would it be possible to determine which CSS properties set on components's root element define external-layout and therefore be **"public"** ? Let's try.

#### External-layout properties

External layout CSS (only the root element):
* `position` ([see](CSS-RESPONSIBILITY.md#position-property))
* `display` ([see](CSS-RESPONSIBILITY.md#display-property))
* `width`, `height` ([see 1](CSS-RESPONSIBILITY.md#fixed-size), [see 2](CSS-RESPONSIBILITY.md#box-sizing))
* `margin`, `margin-*`
* `flex`, `flex-grow`, `flex-shrink`, `flex-basis`, `order`, `align-self`
* `left`, `right`, `top`, `bottom`
* `z-index`

None of those properties are inherited by default, therefore there's no danger of affecting the internal layout ([see W3C CSS 2.1](https://www.w3.org/TR/CSS21/propidx.html)).

#### Position property

`position` is associated with external-layout, although using `position:static` may affect internal-layout in a great way, whenever internal elements need a context for `absolute` positioning. Let's consider `position:static` harmful for a component root and avoid it alltogether ([see](CSS-IMPLICITY.md#css-positioning---problem)).

#### Display property

`display` property is tricky, because it may define both internal and external layout of the same component, both set on its root element, for example:

* `display:flex` - external->`block`, internal->`flex`
* `display:inline-flex` - external->`inline`, internal->`flex`
* `display:block` - external->`block`, internal->`block`
* `display:inline-block` - external->`inline`, internal->`block`
* `display:table` - external->`block`, internal->`table`
* `display:inline-table` - external->`inline`, internal->`table`

Therefore, whenever a component sets the external-layout of its children, it may overwrite their internal-layout settings (`display` property), which may not be desirable.

#### Fixed size

It may happen that a component requires a fixed size, or limited size, because otherwise size setting could break the internal layout. One solution would be to keep `width`, `height` unrestricted for external-layout, and define `max-width`, `max-height`, `min-width`, `min-height` as internal-layout constraints.

#### Box-sizing

With `box-sizing:content-box` which is default, it's impossible to decouple `width`/`height` which define external-layout and `padding`/`border` which are more associated with internal-layout. It would be a good practice to always use `box-sizing:border-box`.

#### Internal-layout properties

Internal-layout `CSS` would include:
- all layout `CSS` defined for internal elements
- the following `CSS` properties defined for the root element:
    - `display` ([see](CSS-RESPONSIBILITY.md#display-property))
    - `flex-direction`, `flex-wrap`, `flex-flow`, `justify-content`, `align-items`, `align-content`
    - `border`, `padding`, `border-*`, `padding-*`
    - `max-width`, `max-height`, `min-width`, `min-height`
    - `box-sizing`
    - `float`, `clear`
    - `overflow`, `overflow-*`

## Look

For components to be useful in many contexts, it should be possible to set their look according to the application styling-theme.

Look-CSS tries to answer the questions: *how pretty?*, *what style?*, *what colors?*

Look is usually defined by the following CSS properties:
- `background`, `background-*`
- `color`
- `outline`, `outline-*`
- `box-shadow`
- `cursor`
- `font`, `font-*`, `text-*`, `letter-spacing`, `line-height`, `word-*`
- `list-*`
- `animation`, `animation-*`, `transition-*`
- `transform`, `transform-*`
- `user-select`
- `opacity`
- `visibility`

### Components are not responsible for their look

**Native HTML5 elements** keep their own look-CSS minimal, setting only some defaults and leaving this responsibility to higher-level component (usually on the application-level).

That approach comes with many advantages:
- makes components highly reusable in applications with different styling-themes
- makes possible to switch/modify styling theme, keeping components untouched
- prevents coupling unrelated components by such trivial thing as styling ;)
- limits components' responsibility, contributing to code clarity and scalability

Let's see how it would look like for custom components.

Here's an example of `MyInput` component (template in React) with minimal CSS:

```html
<div className={'MyInput '+ this.props.className}>
    <label className="MyInput-label">{this.props.label}</label>
    <input className="MyInput-text" type="text" value="{this.state.value}" onChange="{this.update}">
    <button className="MyInput-clear" onClick="{this.clear}">X</button>
</div>
```

```css
.MyInput {
    all: initial;
    display: block;
    position: relative;
    box-sizing: border-box;
}
.MyInput-label {}
.MyInput-text {}
.MyInput-clear {}
```

It would be used in the parent component `MyPerson`:

```html
<div className={'MyPerson ' + this.props.className}>
    <MyInput className="MyPerson-name" label="Name" />
    <MyInput className="MyPerson-address" label="Address" />
</div>
```

```css
.MyPerson {
    all: initial;
    display: block;
    position: relative;
    box-sizing: border-box;
}
.MyPerson-name {}
.MyPerson-address {}
```

And all would be present in the root component `MyApp`:

```html
<div className={'MyApp ' + this.props.className}>
    <h1>Enter your name and address</h1>
    <MyPerson />
</div>
```

```js
ReactDOM.render(<App className="nextgen" />, document.getElementById('root'));
```

```css
.MyApp {
    all: initial;
    display: block;
    position: relative;
    box-sizing: border-box;
}
```

The components' look (as part of styling theme `nextgen`) is defined on the level of the application root component `MyApp`:

```css
/* MyInput */
.MyApp.nextgen .MyInput {
    font-size: 10px;
}
.MyApp.nextgen .MyInput .MyInput-clear {
    background-color: lightgrey;
    color: black;
}
.MyApp.nextgen .MyInput .MyInput-text_empty {
    background-color: mistyrose;
}

/* MyPerson */
.MyApp.nextgen .MyPerson .MyPerson-name.MyInput {
    font-size: 15px;
}
```
