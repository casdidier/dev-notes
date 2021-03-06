# Layout #TODO

```css
position: sticky;
```

```keep the footer at the bottom

https://codepen.io/5t3ph/pen/abvboxz

```css
body {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

footer {
  margin-top: auto;
}

// Optional
main {
  margin: 0 auto;
  // or: align-self: center
  max-width: 80ch;
}

```

```Equal Height Elements: Flexbox vs. Grid
https://moderncss.dev/equal-height-elements-flexbox-vs-grid/

```css
.flexbox {
  display: flex;

  // Ensure content elements fill up the .column
  .element {
    height: 100%;
  }
}



.grid {
  display: grid;
  grid-auto-flow: column;

  // Ensure content elements fill up the .column
  .element {
    height: 100%;
  }
}

```


Logical Properties

```css
margin-block-start, padding-inline-end, etc.

aspect-ratio

content-visibility

```

## single line Layout

[https://1linelayouts.glitch.me/]
[https://www.youtube.com/watch?v=qm0IfG1GyZU&list=WL&index=163]

### super centered

```html
<div class="parent blue">
  <div class="box coral" contenteditable>:)</div>
</div>
```

```css
.ex1 .parent {
  display: grid;
  place-items: center;
}
```

# Shapes & Graphics #TODO

```css
object-fit

clip-path

mix-blend-mode

backdrop-filter

```

# Interactions

```css
overscroll-behavior

overflow-anchor

touch-action

pointer-events


```

# Typography

Web fonts (@font-face)

Variable fonts

## Line breaking properties

```css

overflow-wrap, word-break, line-break, hyphens


font-variant-*

initial-letter

font-variant-numeric

font-display

line-clamp

```

# Animations & Transforms

## CSS Transitions

## CSS Transforms

## CSS Animations

```css
perspective


```

# Media queries

```css
prefers-reduced-motion

prefers-color-scheme

color-gamut


```

# CSS Comparison Functions

```css
min(), max(), and clamp()

```

# Units & Selectors

## Units

## Pseudo elements

````css
::first-line
::after

::before```

## Combinators

```css
div span (descendant)

div > span (child)```


## Tree / Document Structure
```css
:root

:empty

:not()

:nth-child()

:nth-last-child()

:first-child

:last-child```

## Attributes

```css
div[foo] (Presence)

div[foo="bar"] (Equality)

div[foo^="bar"] (Starts with)

div[foo$="bar"] (Ends with)

div[foo~="bar"] (Contains word)

div[foo*="bar"] (Contains substring)

````

## Links/URLs

```css
:any-link

:link and :visited

:local-link

:target

```

## Interactions

```css
:hover

:active

:focus

:focus-within

:focus-visible

```

## Forms Controls

```css
:enabled and :disabled

:read-only and :read-write

:placeholder-shown

:default

:checked

:indeterminate

:valid and :invalid

:user-invalid

:in-range and :out-of-range

:required and :optional



```

## CSS-in-JS

Styled Components
JSS
Styled JSX
Emotion
CSS Modules

## Utilities

Stylelint

PurgeCSS

PurifyCSS

## CSS Proficiency

Virtually no knowledge of CSS

Using CSS frameworks and tweaking existing styles

Knowing specificity rules, being able to create layouts

Mastering animations, interactions, transitions, etc.

Able to style an entire front-end from scratch following a consistent methodology


## Common issues
https://www.smashingmagazine.com/2018/12/common-css-issues-front-end-projects/

#### RESSOURCES

[CSS history](https://medium.com/actualize-network/modern-css-explained-for-dinosaurs-5226febe3525)
