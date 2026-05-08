# wysihtml

wysihtml is an extended and less strict approach on xing/wysihtml5 open source rich text editor based on HTML5 technology.
The code is completely library agnostic: No jQuery, Prototype or similar is required.

This project was initiated and is supported by the [XING AG](https://www.xing.com). Thanks!

## Features

* Auto linking of urls as-you-type
* Generates valid and semantic HTML5 markup (no `<font>` tags)
* Can use class-names instead of inline styles
* Unifies line-break handling across browsers (hitting enter will create `<br>` instead of `<p>` or `<div>`)
* Auto-parses content inserted via copy & paste (from Word, Powerpoint, PDF, other web pages, ...)
* Converts invalid or unknown html tags into valid/similar tags
* Source code view for users with HTML skills
* Uses sandboxed iframes in order to prevent identity theft through XSS
* Editor inherits styles and attributes (placeholder, autofocus, ...) from original textarea (you only have to style one element)
* Speech-input for Chrome

Extended features not present in xing/wysihtml5:

* Can be used without iframe sandbox when initiated on div instead of textarea
* Blocking of image drag drop in editable is removed
* Table insertion management and cell merging commands
* Improved parser with options to: unwrap tag instead of remove, keep defined styles, complex object type definitions for allowing elements.
* Ability to add uneditable area inside editor text flow (useful when building modules like video tools, advanced image editor etc.)
* Improved formatblock handling
* Ability for user to remove formating with only collapsed caret. (without having to select exactly whole text)
* Improved speed
* Anchor creting and removing logic changed to more universal
* Default build is without internal toolbar functions and build with "-toolbar" suffix contains default toolbar functions

## Browser Support

The rich text editing interface is supported in IE8+, FF 3.5+, Safari 4+, Safari on iOS 5+, Opera 11+ and Chrome.
**Graceful Degradation:** Users with other browsers will see the textarea and are still able to write plain HTML by themselves.

## Companies using wysihtml5

* [Basecamp](http://basecamp.com) - Leading web-based project management and collaboration tool
* [XING](https://www.xing.com) - Business Social Network with more than 12 million members
* [Qype](http://www.qype.com) - Largest user-generated local review site in Europe
* and many more ...

## Research

Before starting wysihtml5 we spent a lot of time investigating the different browsers and their behaviors.

Check this repository: https://github.com/tiff/wysihtml5-tests

A compatibility table for rich text query commands can be found here: http://tifftiff.de/contenteditable/compliance_test.html

A pure native rich text editor with HTML validator and live source preview is here: http://tifftiff.de/contenteditable/editor.html

## Development

wysihtml5 can be built using Grunt. Installation instructions for [Grunt can be found here](http://gruntjs.com/getting-started). Once you have it installed, wysihtml5 can be built by simply running

    npm install
    npx grunt

This builds both minified and development versions, including one with toolbar support.

## Customer.io Usage

This is Customer.io's fork of [Voog/wysihtml](https://github.com/Voog/wysihtml). The [UI app](https://github.com/customerio/ui) consumes this library as a git dependency via tags:

```json
"wysihtml5x": "customerio/wysihtml5#cio-v1.2"
```

### Tags

Customer.io releases are managed through `cio-*` tags (not branches):

| Tag | Date | Description |
|---|---|---|
| `cio-v1.0` | 2014-08-06 | Initial fork with liquid template variable support |
| `cio-v1.1` | 2014-12-17 | Build update |
| `cio-v1.2` | 2026-05-08 | Security fixes: upgraded grunt to 1.6, resolved 40 dependency vulnerabilities |

### How changes are managed

1. **Create a branch** from the latest `cio-*` tag (not `main` — the `main` branch has diverged from the tagged releases):
   ```bash
   git checkout -b my-fix cio-v1.2
   ```

2. **Make changes and rebuild dist**:
   ```bash
   npm install
   npx grunt
   ```

3. **Verify the non-minified dist files** (`dist/wysihtml5x.js`, `dist/wysihtml5x-toolbar.js`) are unchanged if you only modified dependencies. The UI app imports the non-minified toolbar file.

4. **Commit and push** the branch (include dist files in the commit).

5. **Create a new tag** with the next version:
   ```bash
   git tag -a cio-v1.3 -m "Description of changes"
   git push origin cio-v1.3
   ```

6. **Update the UI app** (`customerio/ui`):
   - Update `package.json`: `"wysihtml5x": "customerio/wysihtml5#cio-v1.3"`
   - Run `npm install` to update `package-lock.json`
   - Commit both files

### Local testing

To test changes locally before creating a tag:

1. Point the UI app to your local clone:
   ```json
   "wysihtml5x": "file:../wysihtml5"
   ```
2. Run `npm install` in the UI app
3. Start the UI app and test the rich text editor
4. Once verified, revert to the tag reference and create the tag

### How the UI app loads this library

The UI app imports two files in `ember-cli-build.js`:
- `node_modules/wysihtml5x/dist/wysihtml5x-toolbar.js` (editor + toolbar)
- `node_modules/wysihtml5x/parser_rules/advanced.js` (HTML sanitization rules)

These are loaded as global scripts (not ES modules), making `wysihtml5` available on the `window` object. The UI app then wraps it with Ember components (`cio-wysihtml5-editor` for full-featured editing, `wysihtml5-editor` for simple drafts).

## Contributors

See the [list of contributors here](https://github.com/Voog/wysihtml/graphs/contributors).
