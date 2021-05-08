# CanvasTextInput

Canvas Text Input is a library that allows you to quickly and easily create text input boxes inside of an HTML canvas element. I recently had a great need for a tool like this and realized that it didn't exist, so I created my own and felt that I should share it so that others could find use for it too and not have to create it from scratch like I did.

There was a library by Goldfire which attempted to accomplish the same thing my library does, called CanvasInput. His does have more features than mine (such as tabbing through input boxes), but for one reason or another his has a terrible flicker constantly while you use it, and it also runs at about 1-2 frames per second on my machine, causing it to miss over half of the inputted characters. Overall, it's a cool idea, but it's incredibly unuseable and outdated. Mine, on the other hand, has less fancy features and overall worse-looking styling, but it works much much better and never misses any input, runs at a completely consistent frame rate, doesn't ever flicker a single time, and is overall much more reliable. It is certainly ready for use in production and placing it side-by-side with an actual input element you can hardly tell any difference. Overall I'm very happy with it and I hope you will be too. Please give it a try!

## Getting setup

Getting CanvasTextInput setup in your project is incredibly simple. The entire library exists inside of [one single file](https://github.com/OOPS-Studio/CanvasTextInput/blob/main/canvastextinput.js). You're free to copy the contents of that file into a file of your own and import and use that, however you can also use JSDelivr like so:
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
        <script src="textinput.js"></script>
        
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

Once you have Canvas Text Input setup in your project it's very simple to use. Let's learn the basics and then we'll get into the big list of everything that it can do.

### Setup a shell for all your inputs to run in

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

Now that you have the shell setup, you can very quickly and easily create and render new Canvas Text Inputs.

To create a new Canvas Text Input, use the `TextInput` constructor. The constructor takes in 3 required arguments and one optional requirement, like so: `new TextInput(canvas,x,y,*style)`
* `canvas` should be a the canvas element that you want this Canvas Text Input to exist on.
* `x` should be the x position that you want this Canvas Text Input to render at. (left edge)
* `y` should be the y position that you want this Canvas Text Input to render at. (top edge)
* `*style` is an optional argument. We'll look into this more later. All you need to know now is that you can pass in an object that defines the style of your Canvas Text Input. For now we'll leave it blank and let this Canvas Text Input render with default styling.

Once you have your Canvas Text Input created, go ahead and store it in a variable. You can call this variable anything that you'd like, but in this little walkthrough I'll call mine `input1` by saying `let input1 = new TextInput(canvas,x,y);`

Now let's render our input element! To render our Canvas Text Input, we simply need to call `input1.render();` every frame. When we want our input to stop rendering for any length of time, simply stop calling `.render()` on it and begin again when you'd like it to continue rendering.












