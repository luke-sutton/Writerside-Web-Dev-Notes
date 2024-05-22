# Directives

### Local Custom Directives

To create a custom directive you need to create a const and name it in camel case beginning with a v,
then pass in the action to be performed -

```Javascript
const vAutoFocus = {
  mounted: (el) => {
    el.focus()
  }
}
```

Then it can be used in your template the same way as any other directive (such as v-if, v-model etc.),
The name is auto converted to be used in kebab case -

```Javascript
<input v-model="counterData.title" type="text" v-auto-focus>
```

### Global Custom Directives

To be able to use a custom directive globally they should be placed in their own file in a folder
called directives. Each file should be named with the name of the custom directive.

So for the above example within a folder title directives you would create a file named vAutoFocus.js.
The code can then be placed in the file with an export preceding it -

```Javascript
export const vAutoFocus = {
    mounted: (el) => {
        el.focus()
    }
}
```

The directive will then need to be imported to use it within your chosen template -

```Javascript
import { vAutoFocus } from "@/directives/vAutoFocus.js";
```