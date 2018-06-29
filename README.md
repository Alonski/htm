# HTM (Hyperscript Tagged Markup)

<img src="https://i.imgur.com/09ih11e.jpg" align="center" width="715">

`htm` is an implementation of JSX-like syntax in plain JavaScript, using [Tagged Template Literals].
It lets your build apps using Preact/React/etc directly in the browser.
JSX can be converted to `htm` with only a few tiny modifications.
Templates are parsed by the browser's HTML parser and cached, achieving minimal overhead.

`htm` is just _650 bytes_ standalone, or **only 500 bytes** when used with Preact! _(through the magic of gzip 🌈)_

## Syntax: Like JSX but more lit

The syntax is inspired by `lit-html`, but includes features familiar to anyone who works with JSX:

- Rest spread: `<div ...${props}>`
- Self-closing tags: `<div />
- Components: `<${Foo}>` _(where `Foo` is a component reference)_
- Boolean attributes: `<div draggable />`

## Syntax: better than JSX?

`htm` actually takes the JSX-style syntax a couple steps further!
Here's some ergonomic features you get for free that aren't present in JSX:

- HTML's optional quotes: `<div class=foo>`
- HTML's self-closing tags: `<img src=${url}>`
- Optional end-tags: `<section><h1>this is the whole template!`
- Convenient implicit end-tags: `<${Footer}>footer content<//>`
- Support for HTML comments: `<div><!-- don't delete this! --></div>`

## Project Status

The original goal with `htm` was to create a wrapper around Preact that felt natural for use untranspiled in the browser.

I wanted to use Preact, but I wanted to eschew build tooling and use ES Modules. That meant I couldn't use JSX, leaving only [Tagged Template Literals] in its place. So I wrote this library to patch up the differences between the two as much as possible. As it turns out, the technique is framework-agnostic, so it should work great with most Virtual DOM libraries.

**Note: `htm` isn't public yet. If you're here it's because I wanted to show you, please don't share it.**

## Installation

Since `htm` isn't yet published, you must install/download/hotlink it from Gist instead.

**For npm:**

```js
npm i -D git@gist.github.com:ce062586e3fc7247dbb9ccc4f6acc4e1.git
```

**To hotlink:**

```js
import { html, render } from 'https://rawgit.com/developit/ce062586e3fc7247dbb9ccc4f6acc4e1/raw/preact-html.mjs'
```

## Example

Curious to see what it all looks like?
Here's a working app! It's just an HTML file, there is no build or tooling. You can edit it with nano.

```html
<!DOCTYPE html>
<html lang="en">
  <body>
    <script type="module">
      import { html, Component, render } from 'preact-html';
  
      class App extends Component {
        addTodo() {
          const { todos } = this.state;
          this.setState({ todos: todos.concat(`Item ${todos.length}`) });
        }
        render({ page }, { todos = [] }) {
          return html`
            <div class="app">
              <${Header} name="MyApp: ${page}" />
              <ul>
                ${todos.map(todo => html`
                  <li>{todo}</li>
                `)}
              </ul>
              <button onClick=${this.addTodo.bind(this)}>Add Todo</button>
              <${Footer}>footer content here<//>
            </div>
          `;
        }
      }
  
      render(html`<${App} page="To-Do's" />`, document.body);
    </script>
  </body>
</html>
```

How nifty is that?

[Tagged Template Literals]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#Tagged_templates