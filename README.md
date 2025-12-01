### whether deno, bun and node break with solidjs streaming using both the actual nitro preset intended for the runtime and `"node-server"`

**nitro 2:**

bun with bun: yes

deno with deno: yes

bun with node: no

deno with node: no

node: no

**nitro 3:**

bun with node: no

bun with bun: yes

deno with node: yes

deno with deno: yes

node: yes

however, if you use a standalone deno server:

```ts
import { renderToStream, ssr, ssrHydrationKey } from "solid-js/web";
var _tmpl$ = ["<h1", ">Home</h1>"];
function App() {
  return ssr(_tmpl$, ssrHydrationKey());
}

Deno.serve((req) => {
  const { readable, writable } = new TransformStream();
  renderToStream(App).pipeTo(writable);
  return new Response(readable);
});
```

or `srvx` in node:

```ts
import { renderToStream, ssr, ssrHydrationKey } from "solid-js/web";
import { serve } from "srvx";
var _tmpl$ = ["<h1", ">Home</h1>"];
function App() {
  return ssr(_tmpl$, ssrHydrationKey());
}

serve({
  fetch(req) {
    const { readable, writable } = new TransformStream();
    renderToStream(App).pipeTo(writable);
    return new Response(readable);
  },
});
```

it doesn't break.

if i got something wrong, please make a PR to correct me.

more info:

https://github.com/denoland/deno/issues/30226

https://github.com/nodejs/node/issues/60827

https://github.com/oven-sh/bun/issues/18228
