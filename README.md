# CanvasTextInput

Canvas Text Input is a library that allows you to quickly and easily create text input boxes inside of an HTML canvas element. I recently had a great need for a tool like this and realized that it didn't exist, so I created my own and felt that I should share it so that others could find use for it too and not have to create it from scratch like I did.

There was a library by Goldfire which attempted to accomplish the same thing my library does, called CanvasInput. His does have more features than mine (such as tabbing through input boxes), but for one reason or another his has a terrible flicker constantly while you use it, and it also runs at about 1-2 frames per second on my machine, causing it to miss over half of the inputted characters. Additionally, the scrolling on his looks terrible (the text moves back and forth constantly while typing), selecting one of his input elements causes the page to automatically scroll it off the bottom so that you can't even see what you're typing, and his doesn't have any support for arrow keys. (If you go to his GitHub repo you can see a list of 26 different known issues and bugs which are _still open,_ meaning that they are yet to be resolved after 4 years) Overall, it's a cool idea, but it's incredibly unuseable and outdated. Mine, on the other hand, has less fancy features and overall worse-looking styling, but it works much much better and never misses any input, runs at a completely consistent frame rate, doesn't ever flicker a single time, scrolls smoothly, consistently, and nicely, and is overall much more reliable than Goldfire's. Mine is certainly ready for use in production, and placing it side-by-side with an actual input element you can hardly tell any difference. Overall I'm very happy with it and I hope you will be too. Please give it a try!

## Getting set up

Getting CanvasTextInput set up in your project is incredibly simple. The entire library exists inside of [one single file](https://github.com/OOPS-Studio/CanvasTextInput/blob/main/canvastextinput.js). You're free to copy the contents of that file into a file of your own and import and use that, however you can also use JSDelivr like so:
```
<script src="https://cdn.jsdelivr.net/gh/OOPS-Studio/CanvasTextInput/canvastextinput.js"></script>
```
or, for a minified version:
```
<script src="https://cdn.jsdelivr.net/gh/OOPS-Studio/CanvasTextInput/canvastextinput.min.js"></script>
```

Once you have Canvas Text Input imported, you can get a simple demo running like so:
```
<!DOCTYPE html>
<html>
    <head>
        <title>Canvas Text Input Demo</title>
    </head>
    <body>
        <script src="https://cdn.jsdelivr.net/gh/OOPS-Studio/CanvasTextInput/canvastextinput.min.js"></script>
        
        <canvas id = "output-canvas" width = "250" height = "120"></canvas>
        
        <script>
            let canvas = document.getElementById("output-canvas");
            let ctx = canvas.getContext("2d");

            let input = new TextInput(canvas,50,50);

            setInterval(function(){
                ctx.fillStyle = "red";
                ctx.fillRect(0,0,250,120);

                input.render();

                if(!TextInput.requestingHover){
                    document.body.style.cursor = "default";
                }
                TextInput.nextFrame();
            },1000 / 60);
        </script>
    </body>
</html>
```

## Feature rundow/Documentation

Once you have Canvas Text Input set up in your project it's very simple to use. Let's learn the basics and then we'll get into the big list of everything that it can do.

### Set up a shell for all your inputs to run in

Canvas Text Input requires a bit of one-time setup to make sure that it's in-sync with everything else you have going on inside your project. Let's walk through customizing Canvas Text Input to your specific project to ensure that it runs perfectly seamlessly at all times.

You'll need to tell each input element which canvas that you would like it to render on and when to render it, however you will also need to do three super simple things to make sure that the class as a whole can understand and fit into your existing project. These things only need to be done once for the whole project (they don't need to be done each time you create a text input), and these things are:
1. Tell Canvas Text Input when each frame is completed. (This allows it to perform a bit of cleanup)
2. Clean up the mouse cursor after each frame.
3. Clear the canvas render between frames

To tell the class when a frame completes, simply use `TextInput.nextFrame();` at the very bottom of your main loop. This will reset it and make it ready for the next frame. (An example of this in use can be seen in the demo above)

To clean up the mouse cursor you simply need to check at the bottom of every frame whether or not you want to return the cursor to default or if you want to leave it as it is. (This prevents flicker) Odds are that you've already done this for your project by keeping track of a variable that tells you whether or not to reset the cursor, resetting that variable at the end of each frame. If so, then that's perfect! You can integrate that directly into Canvas Text Input with no extra work at all. In your if() statement that checks whether or not to reset the cursor, simply include a check for `!TextInput.requestingHover`, and this will prevent your code from automatically overwriting Canvas Text Input's request for a cursor change, removing any flicker.
The reason you need to clean up the mouse cursor manually is because you may have other parts of your project affecting the mouse cursor, and this allows you to control when Canvas Text Input can and cannot mandate the style of the mouse cursor. If you don't affect the style of the cursor annywhere else in your entire project, then you can simply include the following block of code directly above your `TextInput.nextFrame()` call:
```
if(!TextInput.requestingHover){
    document.body.style.cursor = "default";
}
```
(An example of this can be seen in the demo above)

To clear the canvas render between frames, you simply need to render a sheet of pixels over the whole screen. In the demo above this is done by simply drawing a red rectangle over the entirety of the canvas, however you will most likely not need to add anything here because if you're rendering anything onto the canvas then you're probably already clearing it up between frames. If not then you can very very easily do so with 2 lines of code! (As seen in the demo above)

And that's it! Your Canvas Text Input is all set up and ready to be put to use.

### Create and render your first Canvas Text Input

Now that you have the shell set up, you can very quickly and easily create and render new Canvas Text Inputs.

To create a new Canvas Text Input, use the `TextInput` constructor. The constructor takes in 3 required arguments and one optional requirement, like so: `new TextInput(canvas,x,y,*style)`
* `canvas` should be a the canvas element that you want this Canvas Text Input to exist on.
* `x` should be the x position that you want this Canvas Text Input to render at. (left edge)
* `y` should be the y position that you want this Canvas Text Input to render at. (top edge)
* `*style` is an optional argument. We'll look into this more later. All you need to know now is that you can pass in an object that defines the style of your Canvas Text Input. For now we'll leave it blank and let this Canvas Text Input render with default styling.

Once you have your Canvas Text Input created, go ahead and store it in a variable. You can call this variable anything that you'd like, but in this little walkthrough I'll call mine `input1` by saying `let input1 = new TextInput(canvas,x,y);`

Now let's render our input element! To render our Canvas Text Input, we simply need to call `input1.render();` every frame. When we want our input to stop rendering for any length of time, simply stop calling `.render()` on it and begin again when you'd like it to continue rendering.

### How to style your Canvas Text Input

There are two ways of controlling the style of your Canvas Text Input. One way is simply by including a "styling object" inside the constructor like so: `new TextInput(canvas,x,y,{/* This is the styling object */})` and another way is by calling `setStyle()` on your Canvas Text Input, like: `nameOfTextInput.setStyle({/* This is the styling object */});`

A Canvas Text Input's style can be controlled simply by passing in keys with values. So, for example, the default text size is 15, if I wanted to set the text size of this TextInput to 30, I could simply say:
```
nameOfTextInput.setStyle({
    textSize: 30
});
```
or simply add it to the creation of the Canvas Text Input:
```
let nameOfTextInput = new TextInput(canvas,x,y,{
    textSize: 30
});
```
It's as simple as that. A full list of all the styling options and what they do can be found below under the "**Styling properties**" header. For now, just know that this is how you set each individual value.

### Creating multiple Canvas Text Inputs

There is no limit to how many Canvas Text Inputs you can have at once. You can create as many as you'd like and control them each individually, even on seperate canvases (or all on one or two canvases too, the choice is absolutely yours). Simply call `.render()` on whichever Canvas Text Inputs you'd like to render at any given time and you're all set! You don't need to do any extra work to get as many of these running as you'd like. Just follow the same steps as above for each one.

### Listening to events broadcast by your Canvas Text Inputs

Each Canvas Text Input will broadcast events which allow you to keep track of what it's doing, such as when it's selected, when text is inputted into it, etc. A list of all the different events and what they mean and how to use them can be found under the "**Events. What they do and how to use them**" header.

### Miscellaneous other features

There are two other things you can do with your Canvas Text Inputs. I wasn't sure where they really fit in, so here they are!

#### Setting the placeholder of your Canvas Text Input

You can set a placeholder by doing one of the following: You can either set it directly using `nameOfInputElement.placeholder = "";` or pass in `placeholder` as one of the styling options when passign in a styling object. Either way works just fine, you can mix and match them as well. Just use whichever is nicer for you at any given time.

A placeholder is a small peice of text that displays when the Canvas Text Input is empty. It's exactly identacle to the `placeholder` attribute on an HTML input element, use it the same way you'd use that.

#### You can also set an optional setting to automatically highlight all text in a Canvas Text Input when it's clicked

To do so, simply set `highlightAllTextWhenSelected` to `true`. This can be done directly (`nameOfTextInput.highlightAllTextWhenSelected = true;`) or via the styling object. Again, the choice is yours.

When set to true it will cause all text inside to be highlighted when the user clicks inside the Canvas Text Input. When set to false it will cause a insert bar to appear instead. Default is `false`.

# Full documentation

## Techincal properties:
* `.value`: Gets and sets the value of the text inside the Canvas Text Input.
* `.selected`: Is `true` when the Canvas Text Input is selected (or "focused"), and `false` otherwise. Can be set manually to override.
* `.highlightAllTextWhenSelected`: Is `false` by default, meaning that selecting the Canvas Text Input will create an insert marker. If set to `true`, selecting the Canvas Text Input will highlight all text inside.

## Technical methods:
* `.render()`: Renders the Canvas Text Input a single time. Needs to be called every frame to make Canvas Text Input work.
* `.undo()`: Equivilent to the user pressing `ctrl + Z`
* `.redo()`: Equivilent to the user pressing `ctrl + Y`

## Styling properties:

Stying properties are properties that exist inside of the `.style` property of a Canvas Text Input. These properties can be set using the `.setStyle()` method, or by passing them in inside the 4th parameter of the constructor.

A "styling object" is simply an object which contains key/value pairs that dictate the styling options of a Canvas Text Input. An example of a styling object is as below:
```
{
	textSize: 20,
	paddingLeft: 4,
	paddingRight: 4,
	borderRadius: 5,
	width: 150,
	onSelect: {
		borderColor: "red",
		borderWidth: 4
	}
}
```
A special property called `onSelect` can also take in an object which specifies special properties that will be used when the Canvas Text Input is selected. These properties can be set to `false` to make them automatically inherit the non-selected properties. For example, this code block will remove the default styling of the border turning blue when selected and cause the Canvas Text Input to appear identacle to its un-selected styling:
```
nameOfTextInput.setStyle({
	onSelect: {
		borderWidth: false,
		borderColor: false
	}
});
```
And this block of code will make the background color be green when not selected, and turn red when selected:
```
nameOfTextInput.setStyle({
	backgroundColor: "green",
	onSelect: {
		backgroundColor: "red"
	}
});
```

### All possible styling properties and their effects:

* `width` - Default: `150` - The width (in pixels) of the entire text box. (Excluding padding)
* `placeholder` - Default `""` - The text that will display when the Canvas Text Input has no user-inputted text inside it.
* `backgroundColor` - Default: `"white"` - The color of the inside of the Canvas Text Input box.
* `textColor` - Default: `"black"` - The color of the user-inputted text.
* `borderColor` - Default: `"black"` - The color of the border of the Canvas Text Input box.
* `borderWidth` - Default: `2` - The width, in pixels, of the border of the Canvas Text Input box.
* `borderRadius` - Default: `0` - The radius (or "curve") of the corners of the Canvas Text Input box.
* `placeholderColor` - Default: `rgba(0,0,0,0.65)` - The color of the placeholder text that displays when the Canvas Text Input box has no text in it. This placeholder's text can be specified by `placeholder`
* `highlightColor` - Default: `rgb(60,100,255)` - The color of the background of highlighted text inside the Canvas Text Input. Defaults to a strong blue color.
* `highlightedTextColor` - Default: `"white"` - The color of the text being highlighted. Defaults to white to provide high contrast with the blue background.
* `paddingLeft`, `paddingRight`, `paddingTop`, and `paddingBottom` - Default: `0` (all of them default to 0) - The amount of padding (in pixels) that will be between the border of the Canvas Text Input box and the text inside it. One value for each side. (Each one can be set to any number, and you can specify any amount of them without affecting the others.)

#### Styling properties that can be passed into `onSelect`

The following are all of the properites that can be passed to `onSelect`. They behave exactly the same as their counterparts above, however these only apply when the Canvas Text Input is selected.

Setting any of these to `false` causes it to have no effect.

* `backgroundColor` - Default: `false`
* `textColor` - Default: `false`
* `borderColor` - Default: `"rgb(0,150,255)"`
* `borderWidth` - Default: `2.5`
* `borderRadius` - Default: `false`
* `paddingLeft`, `paddingRight`, `paddingTop`, and `paddingBottom` - Default: `false` (all of them default to `false`)

### You can see a demo of some of the ways you can style your Canvas Text Inputs by going [here](http://oops-studio.com/canvastextinputdemo).
(Please note that copying/pasting will not work on the demo site because the web browser silently blocks it. This happens because my site is served over HTTP instead of HTTPS, so your browser won't trust my site. This won't be an actual issue during production, however.)

## Events. What they do and how to use them

Each Canvas Text Input broacasts its own events which tell you what it's doing and when, and allows you to write code that reacts to what it's doing. For example, processing user input when the user presses the `enter` key.

To listen for an event, simply assign a function to the name of the event you want to listen for. As an example, let's print out `"I was clicked!"` when the user clicks our Canvas Text Input. This code would accomplish that for us:
```
nameOfTextInput.onselect = () => {
	console.log("I was clicked!");
};
```
The `onselect` function will automatically be run one time every time the Canvas Text Input is selected.

Some events automatically provide extra data about the event. For example `onkeypress` will pass along a string containing the key that was pressed, and `onpaste` will pass along a boolean saying whether or not the paste was successful for if the user blocked access to the clipboard. This data can be easily collected by simply giving it a name. For example:
```
nameOfTextInput.onkeypress = e => {
	console.log(e);
};
```
This will print out every keystroke that the user makes inside the text input to the console.

### Full list of all events

* `onselect` - Executes one time when the user selects the Canvas Text Input for the first time. Only executes again when the user unselects and reselects the Canvas Text Input.
* `onunselect` - Executes a single time when the user unselects the Canvas Text Input. Exactly the opposite of `onselect`
* `onkeypress` - Passes along a string containing the key that was pressed - Executes once every time the user presses a key while the Canvas Text Input is selected
* `onkeyrelease` - Passes along a string containing the key that was released - Executes once every time the user releases a key while the Canvas Text Input is selected
* `onenter` - Excutes a single time every time the user presses the `enter` key while the Canvas Text Input is selected.
* `oncopy` - Executes a single time every time the user uses `ctrl + C` or `ctrl + X` to attempt copying/cutting text out of the Canvas Text Input.
* `onpaste` - Passes along a boolean containing `true` if the clipboard read was successful and `false` if it was blocked by the browser - Executes a single time when the user uses `ctrl + V` to attempt pasting text into the Canvas Text Input.
