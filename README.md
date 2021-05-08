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
        <script src="https://cdn.jsdelivr.net/gh/OOPS-Studio/CanvasTextInput/canvastextinput.min.js"></script>
        
        <canvas id = "output-canvas" width = "250" height = "120"></canvas>
        
        <script>
            let canvas = document.getElementById("output-canvas");
            let ctx = canvas.getContext("2d");

            let mouseX = 0;
            let mouseY = 0;
            let hover = false;

            let input = new TextInput(canvas,ctx,50,50);

            setInterval(function(){
                ctx.fillStyle = "red";
                ctx.fillRect(0,0,400,400);

                input.render();

                if(!hover && !TextInput.requestingHover){
                    document.body.style.cursor = "default";
                }
                hover = false;
                mouseClicked = false;
                TextInput.nextFrame();
            },1000 / 60);

            canvas.addEventListener("mousemove",e => {
                let rect = e.target.getBoundingClientRect();
                TextInput.giveMouseData(Math.round(e.clientX - rect.left),Math.round(e.clientY - rect.top));
            });
        </script>
    </body>
</html>
```

## Feature rundow/Documentation

Once you have Canvas Text Input setup in your project it's very simple to use.
