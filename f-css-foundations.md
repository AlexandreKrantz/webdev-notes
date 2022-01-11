# CSS Foundations
## [The Cascade of CSS](https://www.theodinproject.com/paths/foundations/courses/foundations/lessons/css-foundations#the-cascade-of-css)

### Specificity
- A CSS declaration that is more specific will take precedence over ones that are less specific. In order of most to least specific:
    -  Inline styles (in the HTML), have the highest specificity compared to selectors
- Specificity will only be taken into account when an element has multiple, conflicting declarations targeting it, sort of like a tie-breaker. 
    - An ID selector will always beat any number of class selectors, 
    - a class selector will always beat any number of type selectors, 
    - and a type selector will always beat any number of anything less specific than it.
- When no declaration has a selector with a higher specificity, a larger amount of a single selector will beat a smaller amount of that same selector.
- You may come across special symbols for the universal selector (*) as well as combinators (+, ~, >, and an empty space). These symbols do not add any specificity in and of themselves.
    - For example: Rule 1 uses a chaining selector (no space) and rule 2 uses a descendant combinator (the empty space). But both rules have two classes and the combinator symbol itself does not add to the specificity.

### Inheritance
- Inheritance refers to certain CSS properties that, when applied to an element, are inherited by that element’s descendants, even if we don’t explicitly write a rule for those descendants. 
- Typography based properties (color, font-size, font-family, etc.) are usually inherited, while most other properties aren’t.
    - However, directly targeting an element with css is always more specific than inheritance from a parent.

### Rule Order
- If specificity ends up being exactly equal, the styling that was declared last takes priority.

## Inspecting HTML and CSS
- The styling rules that are crossed out have been overridden in the stylesheet and are not currently being applied.

## [The Box Model](https://css-tricks.com/the-css-box-model/) (default behavior = content-box)
- Every element is a box (The Box Model). Margin isn't included in the size of a box, but affects other content interacting with the box. The height/width css property doesn't actually refer to the height/width of the entire box.
    - Box width =  width + padding-left + padding-right + border-left + border-right
    - Box height =  height + padding-top + padding-bottom + border-top + border-bottom
- If padding or borders are undeclared, they are either zero (likely if you are using a css reset) or the browser default value (probably not zero especially on form elements that are commonly not reset).
- The default width of a box with static/relative positioning is whatever is left of the current line.
- The default width of a box with absolute positioning is the minimum width to fit the content inside.
- Inline elements are boxes too. Think of them as really really long and skinny rectangles, that just so happen to wrap at every line. They are able to have margin, padding, borders just like any other box. However, line height overrides padding.  

Wanna see every single “box” that makes up a page? Try putting this in the stylesheet temporarily:
``` css
* {
   outline: 1px solid red !important;
}
```
### Margin
- In the box model, margin collapses between two elements that are next to each other. So if you have an element with `margin-right: 60px;` and another element with `margin-left: 70px;` next to each other, the total white space between them will only be 70px. Only the largest margin value is used.
    - If you have a negative margin, its value is combined with the other margin (so you get a subtraction and smaller resultant margin between the two elements). You can think of an element with negative magin as pulling the other element towards it.
    - `auto` value for margin will usually just default to 0. However, if there is available space on its axis it will take up the available space will the margin.
    - Note that a width value of `auto` or `100%` will take up all the available space, leaving `margin: 0 auto` to take up 0px.
    - If the width of a block element is defined explicity and there is extra space `margin: 0 auto` will do horizontal centering of the element (both margin-left and margin-right share the available space).
    - 
- Note: you can't have negative padding.

### [Block and Inline Boxes](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/The_box_model#block_and_inline_boxes)
- Boxes have an outer and inner display type. Outer display type describes how the box will display in relation to its siblings. Inner display type describes how the boxes children will display relative to each other. For example if you make a div it will have outer display type `block` by default. You can then change its inner display type with the rule `display: flex;`, for example. You can also use `display: none;` to completely hide the element and display the page as if it didn't exist.
    - h1 and p elements use `display: block` by default.
- For `display: inline;` elements, the box will not break onto a new line (unlike `block`). Also, the width and height properties will not apply. Vertical padding, margins, and borders will apply but will not cause other inline boxes to move away from the box. Horizontal padding, margins, and borders will apply and will cause other inline boxes to move away from the box.
    - They sit on the same line as the previous element as long as there is space. If there is no more space, they will wrap the overflowing content to a new line.
    - span and a use `display: inline` by default.
    - `display: inline-block` respects width and height properties + padding, margin and border behave the same as for block. However, it doesn't break to a new line and take up full width by default. 
- the span element is like an inline div.

## [Box Sizing](https://css-tricks.com/box-sizing/)
- `box-sizing` has three possible values (content-box, padding-box, and border-box), the most popular value is border-box.
    - With `box-sizing: border-box;` we can change the box model so that the box height/width correspond to the value of the height/width property. Border and padding eat into this height/width but do not add to it.

How to implement universal box sizing:
``` css
*, *:before, *:after {
  box-sizing: border-box;
}
```

## [Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/#flexbox-background)
- "flex container" is the parent element to the "flex items" (usually a div element).
    - You need to set `display: flex;` for this element. 
    - You also set the flex-direction, to establish the direction of the main axis. It has four values (row default= left to right; column default = top to bottom): `flex-direction: row | row-reverse | column | column-reverse;` (default value: row).
    - By default, flex items will all try to fit onto one line (nowrap). You can change that and allow the items to wrap as needed with this property. `flex-wrap: nowrap | wrap | wrap-reverse;` (note: wrap-reverse makes wraps the elements onto multiple lines from bottom to top, but the order of the element on a line still goes in the specified flex-direction).
    - `flex-flow` is a shorthand for the flex-direction and flex-wrap properties. eg: ` flex-flow: column wrap;`
    - justify-content defines how the flex-items are aligned along the main axis. `justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly | ...`. `space-evenly` puts an equal amount of space on each side of all the flex items, including the ends. `space-between` ignores the ends.
        - Items on the main axis are treated as one group, so there are no `justify-items` or `justify-self` properties.
        - `margin-left: auto` can be used to push flex-items of a row to the right. Thisis because auto margins will take up all of the space that they can in their axis.
    - align-items is like justify content but for aligning flex-items on the cross axis. `align-items: stretch | flex-start | flex-end | center | baseline`. The default `stretch` makes the items fill the cross axis (still respects min-width/max-width though).
    - align-content aligns multiple rows of flex items (you can think of each row being a flex item along the cross axis; not that for multiple rows you need wrapping of the items). `  align-content: flex-start | flex-end | center | space-between | space-around | space-evenly | stretch | ...;`
    - `row-gap` and `column-gap` are used to specify a minimum gap betwen flex items and rows (in lenght units). This gap can get bigger due to justify-content/align-items.
- flex layout is based on "flex-flow" directions instead of inline and block.
    - items are either laid out on (1) the "main axis" which goes from "main-start" to "main-end" OR on (2) the "cross axis", which goes from "cross-start" to "cross-end".
- A flex item has a main size, which corresponds to its length along the main axis. It also has a cross size, which is its length in the cross axis dimension. 
    - Override the order of the items in the html with `order: X`, where X is the index of the item (first item with index 0).
    - flex-grow is set to 0 by default; if all items have `flex-grow: 1;` then they will all grow to take up equal amounts of the available space on the row; if one item has `flex-grow: 2;`, then it will grow twice as much as the others and take up twice as much space. Flex-grow will override the justify content rules and put the extra space in the elements themselves. If an item has larger initial size, it will grow the same amount as the other items and still have larger relative size.
    - `flex-shrink` is like flex grow but for elements to shrink. It is only applied if the size of the all flex items is larger than thier parent container and you have `flex-wrap: nowrap`. Otherwise, you continue wrapping to new lines until each flex item has its own line, then begin shrinking items once their flex-basis is greater than the container size.
    - `flex-basis` specifies the items length on the main axis. The default value `auto` means that the defined height/width property will be respected. If you set `flex-basis: 0%;`, then it will ignore the width/height property and grow/shrink freely. If height/width property is not explicitly defined it will fit to the size of the content.
    - `align-self` overrides the default alignment specified by align-items fo that specific flex-item. 

- `flex` property is a shorthand for flex-grow, flex-shrink and flex-basis (in that order). Default value (only if property is defined): `flex: 1 1 0%`. If you defined only `flex: 1`, it would change the flex-basis from the default value of `auto` to `0%`.
- In a column flex-container, flex-basis refers to the height of a flex item. In a row flex-container, flex-basis refers to the width of a flex item.