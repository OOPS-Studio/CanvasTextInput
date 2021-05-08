# CanvasTextInput

Canvas Text Input is a library that allows you to quickly and easily create text input boxes inside of an HTML canvas element. I recently had a great need for a tool like this and realized that it didn't exist, so I created my own and felt that I should share it so that others could find use for it too and not have to create it from scratch like I did.

There was a library by Goldfire which attempted to accomplish the same thing my library does, called CanvasInput. His does have more features than mine (such as tabbing through input boxes), but for one reason or another his has a terrible flicker constantly while you use it, and it also runs at about 1-2 frames per second on my machine, causing it to miss over half of the inputted characters. Overall, it's a cool idea, but it's incredibly unuseable and outdated. Mine, on the other hand, has less fancy features and overall worse-looking styling, but it works much much better and never misses any input, runs at a completely consistent frame rate, doesn't ever flicker a single time, and is overall much more reliable. It is certainly ready for use in production and placing it side-by-side with an actual input element you can hardly tell any difference. Overall I'm very happy with it and I hope you will be too. Please give it a try!

## Getting setup

Getting CanvasTextInput setup in your project is incredibly simple.


### I will give a rundown of the different features, along with some documentation below:


