At the heart of typestyle is the simple `style` function. It is the function you use to interact with CSS.

## `style`
The signature of this function is super simple: 

```ts
style(...objects: NestedCSSProperties[]): string
```

You get a hold of it from TypeStyle:

```ts
import { style } from "typestyle";
``` 

Essentially it allows you to write CSS in a JS or TypeScript (for greater type safety). For example, lets say you want to create some `red` stuff: 

```ts
/** Import */
import {style} from "typestyle";

/** convert a style object to a CSS class name */
const className = style({color: 'red'});
```

You would use the *generated* class name just like you would use some actual class name e.g. with React: 

```ts
/** Import */
import {style} from "typestyle";

/** convert a style object to a CSS class name */
const className = style({color: 'red'});

/** Usage with e.g. React */
const MyText =
  ({text})
    => <div className={className}>
        {text}
      </div>
```

The class name will look something like `f14svl5e`, this is basically a *hash* of the style objects passed to `style`. In the background `style` has gone ahead and also inserted CSS like `.f14svl5e { color: red }` into the document so using this class name with *any* framework has the desired effect of styling the element.

## CSX
We understand that its difficult to get started with CSS in JS without additional guidance. So we also provide a lot of utility style objects in `typestyle/csx` to decrease you rampup. Also these give more semantic names to CSS concepts that are used commonly. More on this later.

## Concept: Mixin

> A mixin is an object that contains properties for reuse

The `style` function can take multiple objects. This makes it easy to reuse simple style objects e.g. 

```ts
const redMaker = {color:'red'};
const alwaysRedClass = style(redMaker);
const greyOnHoverClass = style(
  redMaker,
  {'&:hover':{color: 'grey'}}
);
```

In fact a large number of mixins are provided by `csx` (`import * as csx from 'typestyle/csx'`). e.g. for flexbox we have `csx.flex`, `csx.content`, `csx.vertical` etc. Use them as you would naturally expect e.g. 

```ts
import * as csx from 'typestyle/csx';
import {style} from 'typestyle';

const flexHorizontalGreen = style(
  csx.flex,
  csx.horizontal,
  { backgroundColor: 'green' }
);

/** Sample usage with React */
const Demo = () =>
  <div className={flexHorizontalGreen}>
    <div>One</div>
    <div>Two</div>
    <div>Three</div>
  </div>;
```

## Concept: Interpolation
You can use `&` in any key for the object passed to `style` and its get replaced with the generated class name when its written to CSS. As an example it allows super simple pseudo state (`&:hover`, `&:active`, `&:focus`, `&:disabled`) customization: 

```ts
/** Import */
import {style} from "typestyle";

/** convert a style object to a CSS class name */
const className = style({
  color: 'blue',
  '&:hover': {
    color: 'red'
  }
});
```

or even child selectors for example a nicely spaced vertical layout: 

```ts
/** Import */
import {style} from "typestyle";

/** Share constants in TS! */
const spacing = '5px';

/** style -> className :) */
const className = style({
  '&>*': {
    marginBottom: spacing,
  },
  '&>*:last-child': {
    marginBottom: '0px',
  }
});
```

> Note: ^ if this CSS looks complex to you, I don't blame you. That's why it in a nice csx mixin e.g. `csx.verticallySpaced(10)`. More on this function when we look at csx box functions later in the book.

With `style` out of the way lets jump to come core CSS tips.