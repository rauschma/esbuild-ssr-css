# esbuild-ssr-css

My use case:

* Use CSS from TypeScript for server-side rendering.
* Thus, this is related to CSS modules but it must work with TypeScript and Node.js (in addition to during bundling).
* CSS names (of IDs and classes) should be checked statically.

I’m still in the process of thinking this fully through. Thus, it’s possible I made mistakes somewhere.

## Example code

Example – files:

```
component.tsx
component.css.json
component.css
```

`component.ts`:

```ts
import { render } from 'preact-render-to-string';
import css from './component.css.json' with { type: 'json' };

console.log(render(
  <div id={css.warning}></div>
));
```

`component.css.json`:

```json
{
  "warning": "x1bz9l0f"
}
```

`component.css`:

```css
#warning {
  background-color: red;
}
```

## What an esbuild plugin would have to do

The plugin would have to forward `component.css.json` to two loaders:

* `json`: Treat it like a normal JSON import so that we have access to CSS IDs and classes at runtime (in the browser).
* `css`: Bundle all the CSS fragments into a single CSS file.

## How would `component.css.json` be generated?

* Initially manually.
* Soon after: automatically, somehow. There are several libraries out there that can be used.

## Benefits of this approach

* Node.js is fine with the JSON imports – in contrast to importing CSS, which doesn’t work for this use case.
* TypeScript warns us during editing if we get CSS names wrong.