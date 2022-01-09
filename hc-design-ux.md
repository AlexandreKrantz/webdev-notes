# Design and UX
## Introduction
- User Experience is really about focusing on three things – can the user get done what they are trying to do effectively, efficiently, and with satisfaction.
- It starts with making your site fast and reliable. Then you need to properly set up the structure and information architecture of the page so users have a logical schema for navigation. Then you will build your user interfaces for optimal user experience. Only then, finally, can you worry about aesthetics.
### [Startups, this is how design works.](https://startupsthisishowdesignworks.com/)
- Design is a method for problem solving. Good design seem "undesigned", because they are so obvious and effective. Good design is often as little design as possible, yet makes the product understandable, useful, unobtrusive, long-lasting, thorough, aesthetic, honest and innovative.
- Good design is key for creating products that customers will love.
### [UX 101](https://web.archive.org/web/20190825035454/https://www.homestead.com/blog/06/2013/ux-101-what-user-experience-infographic#.XWIGlVkRc2w)
- Good UX optimizes customer experience so that they reach their goal more fluidly. It helps avoid the creation of unnecessary problems that developers/support have to solve.
- Are customers every confused? If so, this is probably a UX issue. Ask yourself how the information architecture can be better organized.
### [What is UX?](https://www.smashingmagazine.com/2010/10/what-is-user-experience-design-overview-tools-and-resources/)
- User experience (abbreviated as UX) is how a person feels when interfacing with a system. 
- “Does this website give me value? Is it easy to use? Is it pleasant to use?” These are the questions that run through the minds of visitors as they interact with our products, and they form the basis of their decisions on whether to become regular users. User experience design is all about striving to make them answer “Yes” to all of those questions. 
- Before user-centered design, people just designed websites based on what they thought worked. There was no science or methodology behind design choices. The feelings of users was not considered. It was based on the demands of the client, rather than the best possible experience for the end user.
- Systems that involve a myriad of user tasks must be perceived as being valuable, pleasant and efficient. In this case it is worth doing a multi-member study on the UX.
- Usability is about the user-friendliness and efficiency of the interface. Usability is big part of the user experience and plays a major role in experiences that are effective and pleasant, but then human factors science, psychology, information architecture and user-centered design principles also play major roles.
- UX design efficacy is hard to measure using analytics alone. Instead, we have to tease out the results indirectly by analyzing revenue levels, page views, before-and-after surveys of users and the like. Less quantitative, more qualitative.
- surveys and A/B testing can test UX hypotheses.
### [Visual Hierarchy](https://52weeksofux.com/post/443828775/visual-hierarchy)
- Visual hierarchy suggests there is a proper way to view content visually: in a hierarchical way. In other words, there is a pecking order to things…some content should be viewed first, some second, some third, and on down the line. The most important content is at the top of the hierarchy. It’s the visual element you look at first, which then directs you to what to look at next. 
- Just like a great writer starts with a interesting lead that sends you breathlessly into the next paragraph, a thoughtful designer will efficiently move the viewer from one piece of content to the next. And just as a writer wouldn’t put the climax of their story at the beginning you wouldn’t put the call to action button at top of a landing page. That would leave the user with no context about why they should take that action. In this way you use hierarchy to tell a story, to provide context that culminates in the action being taken.
- The best visual hierarchies lead users to take the action confidently. They have a clear, obvious order in which to view and act on things, with the most important things first. If you’re creating a sign-up screen, for example, the hierarchy might be made up of a catchy headline, some supporting motivational copy (perhaps features & benefits of using the service), a signup form, and a submit button.
- Weak hierarchies, on the other hand, don’t set appropriate context. They may leave out something important, they may frame their topic incorrectly, or they may place design elements in the wrong order. Weak hierarchies fail to organize information in a helpful way and leave users without a clear idea of what to do or read next.
### CRAP Design Principles
- Contrast, Repetition, Alignment, Proximity
- Contrast is what we notice, and it’s what gives a design its energy. So you should make elements that are not the same clearly different, not just slightly different. 
    - You can achieve contrast in many ways—for example, through the manipulation of space (near and far, empty and filled), through color choices (dark and light, cool and warm), by text selection (serif and sans serif, bold and narrow), by positioning of elements (top and bottom, isolated and grouped), and so on. 
- Repetition of certain design elements in a slide or among a deck of slides will bring a clear sense of unity, consistency, and cohesiveness.
- Most of  templates have been seen many times before so it may be too repetitive and boring for the user. Elements shouldn't be exactly the same every time, they should have subtle unique changes to stay fresh and human.
-  Alignment is about obtaining unity among elements of a single slide. Even elements that are quite far apart on a slide should have a visual connection. The audience may not be conscious of it, but slides that contain elements in alignment look cleaner. And assuming other principles are applied harmoniously as well, your slides should be easier to understand quickly.
- The principle of proximity is about moving things closer or farther apart to achieve a more organized look. The principle says that related items should be grouped together so that they will be viewed as a group, rather than as several unrelated elements. Audiences will assume that items that are not near each other in a design are not closely related. Audiences will naturally tend to group similar items that are near to each other into a single unit. 
    - People should never have to “work” at trying to figure out which caption goes with which graphic or whether or not a line of text is a subtitle or a line of text unrelated to the title. Do not make audiences think.
    - we must be conscious of where our eye goes first when we step back and look at our design. When you look at your slide, notice where your eye is drawn first, second, and so on. What path does your eye take?
## Fonts and Typography
### [Working with Typography](https://learn.shayhowe.com/html-css/working-with-typography/)
- Font = file that contains a typeface. The typeface artistic name for the actual look of the letters.
- Choice of text typeface and color is most important for the legibililty and tone of the page. Use css `color` property to change the color of text.
- The `font-family` property is used to declare which font—as well as which fallback or substitute fonts—should be used to display text. You list the names of the fonts in order of preference, separated by commas. At the end, you specify sans-serif or serif. You should write the font name in quotation marks to be safe.
- The `font-size` property provides the ability to set the size of text using common length values, including pixels, em units, percentages, points, or `font-size` keywords.
- `font-style` is used to make the font either italic or normal.
- `font-weight` accepts either keyword values or numeric values. Keyword values include normal, bold, bolder, lighter, and inherit. Of these keyword values, it is recommended to primarily use normal and bold to change text from normal to bold and vice versa. Rather than using the keyword values bolder or lighter, it’s better to use a numeric values (100-900) for more specific control.
    - Check if typeface comes in every weight. If it doesn't, then it will default to the weight that is closest to the value you input (e.g. jump to bold).
- `font-variant` is used to make the font either small-caps or normal.
- `line-height` is the distance between two lines of text. Accepts length values, including pixels, em units, percentages, etc. The best practice for legibility is to set the `line-height` to around one and a half times our font-size property value. This could be quickly accomplished by setting the line-height to 150%, or just 1.5.
    - Can be used to vertically center a single line of text if you set `height` and `line-height` to the same value. Useful for buttons, etc.
- `font` property is a shorthand that includes all the `font-*` properties.
- The `text-align` property has five values: left, right, center, justify, and inherit. These align the text within an element, not the element itself.
- `text-decoration` has values underline, overline, line-thoough, etc.
- The `text-indent` property can be used to indent the first line of text within an element. Simply specify an indent length.
- The `text-transform` property accepts five values: none, capitalize, uppercase, lowercase, and inherit. The capitalize value will capitalize the first letter of each word, the uppercase value will capitalize every letter, and the lowercase value will make every letter lowercase. Using none will return any of these inherited values back to the original text style.
- `letter-spacing` adjusts space between letters.
- `word-spacing` adjusts space between words.
- To embed a web font, use `@font-face` and set its source to the url and font-family value to its name. Or, for Google fonts, just link their stylesheet in the html (it contains the @font-face declaration). Web fonts must always be reference with quotation marks in your css file.

### [Serif vs. Sans-Serif](https://www.impactplus.com/blog/sans-serif-vs-serif-font-which-should-you-use-when)
- Serif fonts say Traditional, Established, Confident, Elegant and Trustworthy.
    - Main serif fonts: Georgia, Garamond, Times New Roman, Basekerville.
- Sans-Serif say modern, youthful, approachable, clean, fun (and more legible). More relatable and human.
    - Main ones are Helvetica, Open Sans, Proxima Nova, Arial
- Stick to 1-3 fonts total for your brand. But choose fonts with a good amount of contrast. They should have at least one shared quality to stay consistent but be otherwise quite different (for example, same letter height or width or designer). You can make a visual hierarchy with different weights and sizes, different fonts is not always necessary.

### Learning Outcomes
- Why do fonts matter?
- What’s the difference between a serif and sans-serif font?
- What are `font-family` attributes used for?
- How is the active font determined in a `font-family`?
    - If multiple family names are used, the browser will select the first one it finds either embedded on the page using @font-face or installed on the user’s operating system.
    - For font-family there is no specific default or initial value; the initial value always depends on the browser and/or operating system.
- Where does the browser actually get its fonts from?
- Where can you get additional fonts from and how do you get them onto your page?
- What are the disadvantages of using web fonts? Of loading your own?
    - Web fonts can be slow to load, but they provide a more custom look.
- What are the important properties of fonts that you can specify using CSS?

## Project: Design Teardown
### [The Box Model](https://css-tricks.com/the-css-box-model/) (default behavior = content-box)
- Recall that every element is a box (The Box Model). Margin isn't included in the size of a box, but affects other content interacting with the box. The height/width css property doesn't actually refer to the height/width of the entire box.
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

### [Box Sizing](https://css-tricks.com/box-sizing/)
- `box-sizing` has three possible values (content-box, padding-box, and border-box), the most popular value is border-box.
    - With `box-sizing: border-box;` we can change the box model so that the box height/width correspond to the value of the height/width property. Border and padding eat into this height/width but do not add to it.

How to implement universal box sizing:
``` css
*, *:before, *:after {
  box-sizing: border-box;
}
```

### [Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/#flexbox-background)
- "flex container" is the parent element to the "flex items" (usually a div element).
    - You need to set `display: flex;` for this element. 
    - You also set the flex-direction, to establish the direction of the main axis. It has four values (row default= left to right; column default = top to bottom): `flex-direction: row | row-reverse | column | column-reverse;` (default value: row).
    - By default, flex items will all try to fit onto one line (nowrap). You can change that and allow the items to wrap as needed with this property. `flex-wrap: nowrap | wrap | wrap-reverse;` (note: wrap-reverse makes wraps the elements onto multiple lines from bottom to top, but the order of the element on a line still goes in the specified flex-direction).
    - `flex-flow` is a shorthand for the flex-direction and flex-wrap properties. eg: ` flex-flow: column wrap;`
    - justify-content defines how the flex-items are aligned along the main axis. `justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly | ...`
    - align-items is like justify content but for aligning flex-items on the cross axis. `align-items: stretch | flex-start | flex-end | center | baseline`. `stretch` makes the items fill the cross axis (still respects min-width/max-width though).
    - align-content aligns multiple rows of flex items (you can think of each row being a flex item along the cross axis; not that for multiple rows you need wrapping of the items). `  align-content: flex-start | flex-end | center | space-between | space-around | space-evenly | stretch | ...;`
    - row-gap/column-gap is used to specify a minimum gap betwen flex items and rows (in lenght units). This gap can get bigger due to justify-content/align-items.
- flex layout is based on "flex-flow" directions instead of inline and block.
    - items are either laid out on (1) the "main axis" which goes from "main-start" to "main-end" OR on (2) the "cross axis", which goes from "cross-start" to "cross-end".
- A flex item has a main size, which corresponds to its length along the main axis. It also has a cross size, which is its length in the cross axis dimension. 
    - Override the order of the items in the html with `order: X`, where X is the index of the item (first item with index 0).
    - flex-grow is set to 0 by default; if all items have `flex-grow: 1;` then they will all grow to take up the available space on the row; if one item has `flex-grow: 2;`, then it will grow twice as much as the others and take up twice as much space.
    - flex-shrink is like flex grow but for elements to shrink.
    - flex-basis specifies the items length on the main axis. The default value `auto` means "look at my width or height property". So it's better to just leave this alone and use the regular width and height properties.
    - This is the shorthand for flex-grow, flex-shrink and flex-basis combined.
    - align-self overrides the default alignment specified by align-items fo that specific flex-item. 




### Bootstrap Notes (from bootstrap docs)
- Bootstrap grid system is based on flexbox. You use `.row` or `.col` to specify the flex-direction, then you can put any element within it. To make a grid, you but `.col` divs within a `.row`.
    - Rows should always be wrappers for columns when making a grid.
    - Content should always go withing columns.
    - 12 cols available by default in a row before wrapping.
    - There are bootstrap classes for columns that provide the different flexbox properties like justify-content, wrapping, gutters etc.
- container-sm, md, lg, etc affects max-width (before that, width = 100%).
- col-sm, md, lg, etc affects at which point the width will stop being 100%. Eg: col-sm-8 will have width 100% for small screens and then width 8/12 afterwards.
    - you can also combine different stuff, Eg: Columns start at 50% wide on mobile and bump up to 33.3% wide on desktop with `col-6 col-md-4`.
- Hierarchy: container -> row -> col -> content
- 
