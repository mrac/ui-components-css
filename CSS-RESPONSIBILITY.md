## Component responsibility

`CSS` is used to set components' layout and look.

### Layout

Layout is about setting the compoment's place in a web document. In old days before `CSS` has become mature technology, it was mainly accomplished by `HTML` markup (`width`, `height` attributes, excesive usage of `table` element, order of tags).

It answers the questions: *where?*, *how big?*, *how far from other components?*
We can see a web document as 3D space where components may be placed.

Layout is usually defined by the following CSS properties:
- `display`
- `position`
- `flex-*`, `align-*`, `order`, `justify-content`
- `float`, `clear`
- `left`, `right`, `top`, `bottom`
- `margin`, `border`, `padding`
- `width`, `height`, `max-width`, `max-height`, `min-width`, `min-height`
- `box-sizing`
- `z-index`

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

Would it be possible to determine which CSS properties set on components's root element define external layout and therefore be **"public"** ?

External layout CSS (root element):
* `position`
* `display`
* `flex`, `flex-grow`, `flex-shring`, `flex-basis`, `order`, `align-self`
* `left`, `right`, `top`, `bottom`
* `margin`
* `width`, `height`, `max-width`, `max-height`, `min-width`, `min-height`
* `z-index`

### Position property

`position` is associated with external-layout, although using `position:static` may affect internal-layout in a great way, whenever internal elements need a context for positioning. Let's consider `position:static` harmful for a component root and avoid it alltogether.

### Display property

`display` property is tricky, because it may define both internal and external layout of the same component, both set on its root element, for example:

* `display:flex` - external->`block`, internal->`flex`
* `display:inline-flex` - external->`inline`, internal->`flex`
* `display:block` - external->`block`, internal->`block`
* `display:inline-block` - external->`inline`, internal->`block`

Therefore, whenever a component sets the external-layout of its children, it may overwrite their internal-layout settings (`display` property), which may not be desirable.

### Box-sizing

With `box-sizing:content-box` which is default, it's impossible to decouple `width`/`height` which define external-layout and `padding`/`border` which are more associated with internal-layout. It would be a good practice to always use `box-sizing:border-box`.

### Look

For components to be useful in many contexts, it should be possible to set their look according to the application styling-theme.

Look-CSS tries to answer the questions: *how pretty?*, *how fency?*, *what style?*, *what colors?*

Look is usually defined by the following CSS properties:
- `background`, `background-*`
- `color`
- `border`, `border-*`, `outline-*`
- `box-shadow`
- `cursor`
- `font`, `font-*`, `text-*`, `letter-spacing`, `line-height`, `word-*`
- `list-*`
- `animation`, `animation-*`, `transition-*`
- `transform-*`, 
- `user-select`
- `opacity`
- `visibility`

### Components are not responsible for its look

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
    position: relative;
    box-sizing: border-box;
}
.MyPerson-name {}
.MyPerson-address {}
```

The components' look (as part of styling theme) is defined in the application root component `MyApp`:

```html
<div className={'MyApp'}>
    <h1>Enter your name and address</h1>
    <MyPerson>
</div>
```

```css
/* MyApp */
.MyApp {
    all: initial;
    position: relative;
    box-sizing: border-box;
}

/* MyInput */
.MyApp .MyInput {
    font-size: 10px;
}
.MyApp .MyInput .MyInput-clear {
    background-color: lightgrey;
    color: black;
}

/* MyPerson */
.MyApp .MyPerson .MyInput.MyPerson-name {
    font-size: 15px;
}
```
