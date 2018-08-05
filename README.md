#  Xcode IB Auto Layout ifs and buts

OK, so let me play along with the video tutorial
[14 Auto Layout Best Practices for Xcode 9 and iOS 11 (using Storyboards)](https://www.youtube.com/watch?v=nqO11vmzapg) by Paul Solt of supereasyapps, and the series [30 Auto Layout Best Practices](https://blog.supereasyapps.com/30-auto-layout-best-practices/#the-hidden-layout-error-panel) from the same bloke (guy, if you prefer).

From iPhone_Apps_101.zip I picked out `3.2 - Tip Calculator - Begin` project directory, seeing that the Main.storyboard looks about the same as that seen at the start of the video.

Add .gitignore, commit all the uncommitted assets and minor tweaks. It builds successfully.

In IB, add previews for several devices, from the tiny iPhone 4s to the huge iPad 12.9.

### 30 Practices

Let me first study the 30 practices. Sounds less intimidating than 50 shades of whatever.

##### 1. Design for iPhone 8

Done for us by Paul, thanks.

##### 2. Layout Your UI for One iPhone

The starter design is for iPhone 8, so let's stick with it.

##### 3. Undo and Redo

Indeed. Maybe also commit it when it looks like a partial achievement.

##### 4. Relative and Centered Layouts

Aha, sounds like there be a method in the autolayout madness. Being programmers, we like methods.

_Tip: The default spacing between most iOS UI elements is 8 points, and 20 points on the edges._ Sounds good, will sure look good.

_To center content and get more control, you need to add additional subviews (even if these UIView's are transparent or white)._ Also sounds good. Are we going to build some reusable components in this way?

I don't have a dog, so I'm not playing along at this first reading.

So, the top view will stick to top and side edges, but has a fixed height for now. Shall we make it proportinal later?

##### 5. Identify Structural Regions

- _Markup what regions are important to the overall structure._
- _Which regions should stretch?_
- _Which regions should remain fixed (height and width)?_

OK, will do.

##### 6. Understand the Colors

- _Blue: Layout Complete_
- _Orange: Layout Warning_
- _Red: Layout Error_

Easy to get that, but difficult to interpret when there is a horrible mesh (or a mess) of those lines. I have a peeve and a wish (see below).

OK, it will take some work to master
- providing the right number and quality of constraints to get rid of red lines
- handling the flexibility (of text lenghts for example), using the <=/>= constraints, and hugging/stretching ressistance...

##### 7. Work in One Direction

IOW, from the origin at (0,0). It makes sense to work systematically.

_Stop reading this guide and try it on a test app._

OK, I will follow the guide.

And also, I'd like to find somewhere a formal discussion of intrisic sizes  (of images, texts, anything else?), how these can vary and how it impacts the autolayout.



### Apple Xcode+IB peeves and wishes
- It looks like there is no keyboard shortcut to the Preview tab in IB. It takes 3 clicks to get there, once you know where to click.
- About the red diagnostic lines that signify *Layout Error*: why can't we have red lines for conflicts (overspecified)  and say green lines for missing constraints (underspecified)? And a selector for showing only horizontal errors, only vertical ones, and all errors?