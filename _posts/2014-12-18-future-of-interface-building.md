---
layout: post
title:  Future of interface building in Cocoa
date:   2014-12-18 16:00:00
---

The methodology of building interfaces in Cocoa has changed **a lot** over the past few years. There has been a tension between the side of building-interfaces-through-code and the side of building-interfaces-through-Interface Builder. The former gives you far more immediate control over how your interface will appear and what sort of stuff you can do with it, but the price of that is some really long, hairy view code: _hundreds of lines_ of verbose, boilerplate code filled with integer constants and layout math. That is **not** a pretty sight.

There have been attempts to reduce this with CSS-inspired ["stylesheets"](https://github.com/tombenner/nui) for configuring interfaces, but the layout systems in [AppKit] and [UIKit] are just too different from the HTML DOM for such adaptations to really feel natural and work well.

[AppKit]: https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/ObjC_classic/index.html
[UIKit]: https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIKit_Framework/

The alternative is to of course use [Interface Builder], which is what Apple wants you to do. However, Interface Builder has long been a big, clunky system prone to weird behaviors. It also doesn't have the best navigation (if *only* it worked like Adobe Illustrator where one can easily navigate through the hierarchical document with double clicks and the extremely-useful [isolation mode]).

[Interface Builder]: https://developer.apple.com/xcode/interface-builder/
[isolation mode]: http://helpx.adobe.com/illustrator/using/selecting-objects.html#isolate_artwork_for_editing

#### The curse of interactivity

To compound on this struggle we must toss in the fact that most all interfaces must be *interactive*, which introduces a whole new set of demands to what and how we build interfaces. The interfaces-through-code approach requires you to throw in even more minutia-filled animation code, and the Interface Builder-based approach requires you to wade through arcane animation systems. Neither of those are appealing prospects.

Furthermore these interfaces have to accept and/or present information from/to external sources. So now we must add in glue code to handle shuffling that data from the interface to the backend data systems. People have tried to de-clutter this with the [MVVM] approach, but so far that seems to have ended up just multiplying the amount of glue code and introducing extra complexity & opportunity for bugs and/or confusion. Having to deal with model, view model, view controller, and view? There's a lot of layers in that onion, and it's probably going to cause a lot of tears.

[MVVM]: http://www.raywenderlich.com/74106/mvvm-tutorial-with-reactivecocoa-part-1

### A way forward

The good news is that Apple seems to be making great strides in simplifying this whole setup. First off there are the new [IB_DESIGNABLE and IBInspectable](http://inessential.com/2014/11/08/first_go_at_using_ib_designable_and_ibin) systems that allow for far more powerful interface construction and closer tie-in between what you see in Interface Builder, what you write in your code, and what the user ultimately experiences in the application. That is truly great.

Second there is the powerful new [computed property] functionality in Swift. I have been using this a lot recently to bridge the gap between data in the view controller and data in the visual interface. Instead of complicated glue code in the view class or a whole view-model sitting in between, I just use a computed property with a getter and setter in the view class. The view controller then interacts with that computed property, providing a clear separation of concerns between the view controller (backend data layer) and the view (frontend presentation layer). Here's a much-simplified version of this arrangement:

```swift
class ViewController {
  func doingSomething(bar: BazObject) {
    view.bar = bar
  }
  func doingSomethingElse() {
    let bar = view.bar
    greatComplexity(bar)
  }
}

class View {
  get {
    return Bar(baz: textField.text)
  }
  set(bar) {
    textField.text = bar.baz
  }
}
```

[computed property]: https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Properties.html#//apple_ref/doc/uid/TP40014097-CH14-XID_386

Compared to the clunky glue code of the past this feels like a breath of fresh spring air. Now if only we could bring this sort of straightforward controller-view connections and clean code to other environments (looking at you, HTML and JavaScript).
