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

For a practical exercise, I'm taking Paul's initial design of the Tip Calculator, laid out for iPhone 8. It has no autolayout constraints and therefore does not adapt to other screen sizes.

Looking at the design, I see 4 vertical regions:
- TopView (reception-icon)
- PctView (percentages 10/15/20%)
- BillView (your-bill with amount)
- BottomView (calculate-tip button and bottom edge)

I'll try to assign to these regions respectively 30% 40%, 18% ans 12%, total 100% and no tip to add at the end.

Better yet, I'll add 4 views right under the  SafeArea in the list, add different background colors, and manipulate their vertical sizes to match the Paul's UI items.

OK, I added the 4 views, dragging them into the item list, just under the SafeArea, set the colors. Next, I added a chain of vertical spacing constraints (3 bottom->top and 2 to the SafeArea top and bottom). Since my 4 views have no height specified as yet, these constraints are missing.

Commit, so that I can play with the next steps safely.

Just for fun, try to Add Missing Constraints to all: a large number of horizontal and vertical ones added : a horror. Reset (git via Xcode).

Now select my 4 views and ask to add constraints to Selected. It adds horizontal and vertical constraints, pulling in one or two of Paul's UI items which I did not select. As anticipated, it's no good, reset.

Next I will set for each view a vertical size as a proportion of the SafeArea height, 3 of them should suffice, I think.

My proportions based on what I see in the layout will be 0.32, 0.24, 0.24, 0.20.

Take the TopView to SafeArea, set EqualHeight (other 3 views disappear at the bottom), edit it to Multiplier = 0.32, OK.

Take the next 2 views, set+edit EqualHeight to SafeArea height * 0.24, bingo: vertical Missing Constraints are gone.

Note in passing: IB reports missing (or conflicting) constraints only for UI items that have been given at least 1 constraint, not before. One more reason to proceed slowly and methodically, one, or few UI items at a time.

My 4 views have horizontal constraints missing - I could have started there, it should be easy, as I want each of them to take the full width of the SafeArea.

Easy? I have the choice of defining for each of these:
center and width, leading edge and width, leading and trailing edges. I prefer center and width.

Ctrl-drag TopView to SafeArea: the little black dialog proposes Equal Widths and Horizontal Spacing, so I select these (shift key to hold the dialog, thanks Paul). I get Equal Width and TopView.trailing to SafeArea.trailing. Oh, well, I'll keep the Equal Width and change to TopView.leading to SafeArea.leading, just so.

Repeat this for the next 3 views? Or align them horizontally?

Tried to align the 4 views on leading and trailing edges. First view was OK, other 3 had same, but wrong trailing edge. IB aligned all 4 to the wrong trailing edge. Reset (in git).

OK, do it individually, for the 3 remaining views, like I did for the TopView.

Look, no errors!

What a pleasure to select one constraint at a time and view it in 3 ways (scene - views list, design view, Size Inspector).

Bonus: my 4 colored views look good on all 4 previews for devices of different sizes.

Of course, I did not yet deal with the real stuff - Paul's UI items, that go on top of my 4 views.

Start with the simplest case: TopView with its ReceiptIcon. Drag it (in the scene list) to become a subview of the TopView. Ctrl-drag between the two, select both Center * in Container. Done.

Icon is centered in my TopView on all previews. It is just a little too big for the iPhone 4s.I could increase the vertical proportion of the TopView to accommodate.

Wait, what are those orange circles on the Icon? Not an IB warning? Ah, no these are a part of the icon image, perhaps representing small change in Bitcoins. No problem with that.

Which one do I take next?

BillView seems simple, a Label, a TextField and an Image - a boring gray rectangle, but I must make it stretchable.

Time to learn of intrinsic view sizes (texts? images?), stretching and localization issues (Your bill, Ihre Rechnung, Votre addition, ...).

OK, start with those percent items in the PctView? Ah, but those callouts are also images to be made stretchable, perhaps.

But, what did Paul say? Do everything right for the iPhone 8 first, then only deal with other models and their screen sizes.

Working with auxiliary views served me well so far, so I'll create 3 Tip*PctView, each with its Tip Callout ImageView, the % label and the $ label.

Looking at the Callouts, I need to allocate 0.291 of the PctView.width to each Tip*PctView. For simplicity, their height will be same as PctView.height. Color the middle one gray, keep the outer ones white.

Fixed for all 3 Tip*PctView items' height to that of the container and top to the top of the container: no vertical errors.

Fixed for all 3 the width to 0.291 * container.width.
Next, center the middle one in the container, and make them touch each other horizontally.

Done, no more layout errors.

In fact, my PctView plays the role of a horizontal StackView, and perhaps it should be replaced by a StackView.

Now, pull into each of 3 Tip*PctView items their UI items.

I will continue to play the proportional vertical placement game in each Tip*PctView, with the hypothesis that this might facilitate the adaptation to other screen sizes.

Question: is there a way to specify the position of a view as a proportion of the container height? A constraint like A.CenterX = B.CenterX  does have a multiplier - I must investigate whether that would work.

In the meantime, I reproduced the absolute vertical positions of the Callout e.t.c. items as given in Paul's design for iPhone 8, but relative to their containing Tip*PctView, and I centered them horizontally. No layout errors.



### Apple Xcode+IB peeves and wishes
- It looks like there is no keyboard shortcut to the Preview tab in IB. It takes 3 clicks to get there, once you know where to click.
- About the red diagnostic lines that signify *Layout Error*: why can't we have red lines for conflicts (overspecified)  and say green lines for missing constraints (underspecified)? And a selector for showing only horizontal errors, only vertical ones, and all errors?
