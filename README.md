# vue-accessibility-toolbar

A fork of the incredible [Vue-Accessibility-Toolbar](https://hendrixfan.github.io/vue-accessibility-toolbar/).

### Includes other stuff:

 - [OpenDyslexic Font](https://github.com/antijingoist/opendyslexic)
 - Updated Dependencies and such
 - [Material Design Icons](https://materialdesignicons.com/)

### Coming Soon:

 - Big Mouse Cursor
 - Built-in Screen Reader
 - PDF Text-based alternative generator by PDFkit
 - Nuxt.js Support

## Usage

```html
<AccessibilityToolbar></AccessibilityToolbar>
```

```js
import { AccessibilityToolbar } from 'vue-accessibility-toolbar'

export default {
  components: {
    AccessibilityToolbar
  }
}
```

#### Props

- `invert-colors-text` ***String*** (*optional*) `default: 'Invert Colors'`

- `highlight-links-text` ***String*** (*optional*) `default: 'Highlight Links'`

- `grayscale-text` ***String*** (*optional*) `default: 'Desaturate'`

- `accessibile-font-size-text` ***String*** (*optional*) `default: 'Increase Text Size'`

-  dyslexic-font-text ***String*** (*optional*) `default: 'Desaturate'`
