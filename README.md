# w-select

Everything you wish the HTML `<select>` element could do, wrapped up into a lightweight, zero dependency, extensible Vue component.

`w-select` is a feature rich select/dropdown/typeahead component. It provides a default template that fits most use cases for a filterable select dropdown. The component is designed to be as lightweight as possible, while maintaining high standards for accessibility, developer experience, and customization.

- Tagging
- Filtering / Searching
- Select Single/Multiple Options
- Customizable with slots and stylus variables
- ~23kb Total / ~3kb CSS / ~20kb JS

# Documentation

Complete documentation and examples available at https://github.com/weijuer/w-select#readme.

# Install

```
yarn add w-select
```

# or use npm

```
npm install w-select
```

Then, import and register the component:

```
import Vue from "vue";
import wSelect from "w-select";
import "w-select/dist/w-select.css";

Vue.component("w-select", wSelect);
```

The component itself does not include any CSS. You'll need to include it separately:

You can also include vue-select directly in the browser. Check out the documentation for loading from CDN..

# License

MIT

# Keywords

select vue.js

# Keywords

none
