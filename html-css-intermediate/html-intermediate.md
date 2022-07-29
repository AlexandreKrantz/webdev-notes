# Intermediate HTML Concepts
## Emmet
- Emmet is built into VS Code.
- Emmet wrap with abbreviation shortcut: `ctrl+k k`
- Emmet remove tag shortcut: `ctrl+k j`
- You can use the css child element selector to generate nested elements: `ul>li*3` generates 3 li elements inside of a ul.
- Can specify text inside of the elements: `li{some text}`
- Create sibling elements: `header+main+footer`
- Order of operations with parenthesis: `(header>h2)+(main>h1)`

## Form elements basics
- The `<form>` element is a container for wrapping a collection of inputs. Two essential attributes:
    - The `action` attribute takes a URL value that tells the form where it should send its data to be processed.
    - The `method` attribute tells the browser which HTTP request method it should use to submite the form (usually `get` or `post`). This is how it can request/update the server's data.
    - Warning: It's strictly forbidden to nest a form inside another form. Nesting can cause forms to behave in an unpredictable manner, so it is a bad idea.
- _Form control_ elements are the group of elements that the user actually interacts with, including text boxes, dropdowns, checkboxes, etc.
- The `<input>` element accepts user keyboard input.
    - You can restrict the type of input with the `type` attribute. For example: `type="text"`, `type="email`, `type="password"`, `type="number"` or `type="date"`. You can use `type="search"` for search bars (these have a different default styling); `type="tel"` for phone numbers.
    - `placeholder` attribute enables you to put placeholder text.
    - `name` attribute enables us to set the name of the variable containing the input data. 
    - For a required form widget, add the `required` attribute to the HTML tag (no associated value).
    - You can use the `minlength` and `maxlength` attributes to restrict the number of characters allowed.
    - You can use the `min` and `max` attributes to constrain the range of a number input.
    - You can also use the `pattern` attribute to force the user input to match a regular expression.
- For multiline inputs, there is the `<textarea>` element. You can set their initial size with the rows and cols attributes. 
- The `<label for="id-of-form-element">` element is used for accessibility purposes. When the label associated with an input is clicked, the cursor will focus on the input.
	- 
- When a form is submitted, it will create a form object at send it via the specified URL and HTTP request. The properties of the object will correspond to the name attributes of the different form control elements: 
```
"form": {
    "age": "33",
    "first_name": "John",
    "last_name": "Doe"
  },
```
- The `<select>` element renders a dropdown list where users can select an option. Syntactically, select elements have similar markup to unordered lists. The select element wraps `<option>` elements which are the options that can be selected.
    - The `name` attribute is specified in the select element. 
    - All the option elements need to have a `value` attribute. This value will be sent to the server when the form is submitted.
- To create radio buttons, use input elements with `type="radio"` and the same `name` attribute. This way they will be linked together so that only one can be selected at a time. Use the `value` attribute to specify the data sent to the server.
    - We can set the default selected radio button by adding the `checked` attribute to it (without any value).
- To create checkboxes, use input elements with `type="checkbox"` and the same `name` attribute. Use the `value` attribute to specify the data sent to the server. 
    - Use `checked` attribute to check the box by default.
- `<button>` element can have various functionalities when clicked:
    - `type="submit"` submits the form that the button is placed in.
    - `type="reset"` clears all the data the user has entered into the form.
    - `type="button"` is a generic button that we can use how we want.
- The fieldset element is a container element that allows us to group related form inputs into one logical unit. Enclose a group of form-control elements within a `<fieldset>` element.
    - The `<legend>` element is used to give field sets a heading or caption so the user can see what a grouping of inputs is for.
    - A common use-case for these elements is using a fieldset to group radio buttons and using a legend to communicate to the user what each of the options is ultimately for.

## Styling Forms
- To style forms, you've got to override the default styles. This can be tricky (esp with radio buttons, date pickers, etc).
- By default, some form widgets do not inherit font-family and font-size from their parents. You should specify it explicitly. 
    - `font-family: inherit`causes the property value to match the computed value of the property of its parent element.
- Each form widget has different rules for border, padding and margin. It is a good idea to reset these properties and size the elements using `box-sizing: border-box;`.
- Pseudo-classes relevant to forms:
    - `:hover` Selects an element only when it is being hovered over by a mouse pointer.
    - `:focus` Selects an element only when it is focused (i.e. by being tabbed to via the keyboard).
    - `:active` selects an element only when it is being activated (i.e. while it is being clicked on, or when the Return/Enter key is being pressed down in the case of a keyboard activation). 
    - `:required` and `:optional` Targets required or optional form controls. Form controls are optional by default, so you don't really need to use the latter.
    - `:valid` and `:invalid`, and `:in-range` and `:out-of-range`: Target form controls that are valid/invalid according to form validation constraints set on them, or in-range/out-of-range.

## SVGs
- SVGs (Scalable Vector Graphics) are a scalable image format, which means they will easily scale to any size and retain their quality without increasing their filesize. They are often used for icons, and large simple images like placeholders or backgrounds.
- Vector graphics are defined by math instead of a grid of pixels (which we call _rasher graphics_).
- SVGs are defined using XML (aka, “Extensible Markup Language”). This is an HTML-like syntax which is used for lots of things, from APIs, to RSS, to spreadsheet and word editor software.
    - This makes SVGs human readable.
    - The second benefit of XML is that it’s designed to be interoperable with HTML, which means you can put SVG XML code directly in an HTML file, without any changes, and it should display the image. 
- SVGs are efficient for storing simple graphics, but very ineffient for complex photo-realistic images.
- Puting the SVG code in the HTML (inline) is useful if you water to edit it with CSS/JS. Otherwise you can link to the external file within an `<img>` tag or within the css. 
