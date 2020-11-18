## install from powershell

```
iwr https://deno.land/x/install/install.ps1 -useb | iex
```

## Getting started

```ts
deno run https://deno.land/std/examples/welcome.ts
```

```ts
import { serve } from "https://deno.land/std@0.77.0/http/server.ts";
const s = serve({ port: 8000 });
console.log("http://localhost:8000/");
for await (const req of s) {
  req.respond({ body: "Hello World\n" });
}
```
