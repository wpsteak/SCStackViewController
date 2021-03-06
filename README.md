# SCStackViewController

SCStackViewController is a container view controller which allows you to stack other view controllers on the  top/left/bottom/right of the root and build custom transitions between them while providing correct physics and appearance calls. It was build with the following points in mind:

1. Simple to understand and modify
2. Left/right OR top/bottom stacking
3. Correct physics
4. Correct appearance calls
5. Customizable transitions
6. Pagination
7. Customizable interaction area
8. Completion blocks for everything

### Change Log v3.0.1

* Fixed step navigation methods
* Fixed pagination after bouncing
* Exposed visiblePercentageForViewController: method
* Exposed layouterForPosition: method
* Fixed firstResponder warning
* Fixed duplicate symbols when used together with [SCPageViewController](https://github.com/stefanceriu/SCPageViewController)
* Renamed UIViewController category methods to avoid conflicts
* Various tweaks and fixes

### Change Log v3.0.0

* Added a new feature called navigation steps whic represent a relative position in a view controller at which the stack will paginate/stop while scrolling. Useful for defining multiple steps when folding/unfolding a certain view controller. 

- a navigation steps is created using the percentage at which the stack should bounce/paginate (ranges from 0.0 to 1.0) and is assigned on a per viewController basis.
```
SCStackNavigationStep *step = [SCStackNavigationStep navigationStepWithPercentage:0.5f];
[self.stackViewController registerNavigationSteps:@[step] forViewController:leftViewController];
```

- the client can also navigate to any navigation step, registered or not, using:
```
-(void)navigateToStep:(SCStackNavigationStep *)step
       inViewController:(UIViewController *)viewController
               animated:(BOOL)animated
             completion:(void(^)())completion;
```    


* Added option for specifying if the navigation steps are respected when folding, unfolding or both
```
[self.stackViewController setNavigationContaintType:SCStackViewControllerNavigationContraintTypeForward | SCStackViewControllerNavigationContraintTypeReverse];
```

* Interface builder support (used in the demo project)
* Better rotation handling
* Added new layouter which resizes the root view controller instead of offsetting it

![ResizingStackLayouter](https://dl.dropboxusercontent.com/u/12748201/Recordings/v3.0.0/Resizing.gif)

### Change Log v2.1.0

* Added ability to stack view controllers either on top or below the root view controller
* Exposed minimum and maximum number of touches properties
* Switched run loop modes for better integration with other scroll views


### Change Log v2.0.0

* Integrated MOScrollView for better control over scroll animation and duration
* Added pagedNavigation (exposed continuousNavigation property. Defaults to false). Swiping will bounce at every page and multiple swipes will be required to navigate through more than 1 page
* Added delegate for callbacks when a view controller was shown or hidded and when the Stack's scroll content offset was changed
* Fixed pop animation which now goes through the layouters
* Fixed appearance calls so that they inform the correct view controllers and work while scrolling/tracking.
* Proper reversed stacking implementation
* Fixed parallax layouter to work for the first view controllers
* Added iOS 5.x support
* Added documentation
* Exposed animation duration (defaults to 0.25f)
* Exposed timing function (defaults to kCAMediaTimingFunctionEaseInEaseOut)
* Exposed scroll view bounces property (defaults to YES)
* Exposed scroll indicator visiblity control (defaults to not visible)
* Exposed pagingEnabled property
* Exposed root view controller as readonly

## Implementation details

SCStackViewController is build on top of an UIScrollView which gives us the physics we need, content insets for all the 4 positions, callbacks for linking the custom transitions to and easy to build pagination. By overriding the scrollView's shouldReceiveTouch: method we also get the customizable interaction area.

The stack itself relies on layouters to know where to place the stacked controllers at every point. They are build on top of a simple protocol and the demo project contains 6 examples with various effects.

## Examples

##### Plain Stack Layouter
It places the view controllers to their final position and doesn't modify them while dragging.

![PlainStackLayouter](https://dl.dropboxusercontent.com/u/12748201/recordings/v2.1.0/Plain.gif)

##### Reversed Stack Layouter
Reverses the direction used in the plain layouter

![ReversedStackLayouter](https://dl.dropboxusercontent.com/u/12748201/recordings/v2.1.0/Reversed.gif)

##### Sliding Stack Layouter
It reveals every new controller from beneath the previous one through sliding

![SlidingStackLayouter](https://dl.dropboxusercontent.com/u/12748201/recordings/v2.1.0/Sliding.gif)

##### Parallax Stack Layouter
Add a nice parallax effect while revealing the stacked controllers

![ParallaxStackLayouter](https://dl.dropboxusercontent.com/u/12748201/recordings/v2.1.0/Parallax.gif)

##### GoogleMaps Stack Layouter
The effect seen in the Google Maps app when opening the drawer

![GoogleMapsStackLayouter](https://dl.dropboxusercontent.com/u/12748201/recordings/v2.1.0/GoogleMaps.gif)

##### MerryGoRound Stack Layouter
Just something I was playing with.. :)

![MerryGoRoundStackLayouter](https://dl.dropboxusercontent.com/u/12748201/recordings/v2.1.0/MerryGoRound.gif)

## Usage

- Import the stack into your project
```
#import "SCStackViewController.h"
```

- Create a new instance
```
stackViewController = [[SCStackViewController alloc] initWithRootViewController:rootViewController];
```
 
- Set a touch refusal area (optional)
```
[stackViewController setTouchRefusalArea:[UIBezierPath bezierPathWithRect:CGRectInset(self.view.bounds, 50, 50)]]
```
 
- Register layouters
```
id<SCStackLayouterProtocol> layouter = [[SCParallaxStackLayouter alloc] init];
[stackViewController registerLayouter:layouter forPosition:SCStackViewControllerPositionLeft];
```

- Push view controllers
```
[self.stackViewController pushViewController:leftViewController 
        						  atPosition:SCStackViewControllerPositionLeft 
								  	  unfold:NO 
								  	animated:NO 
								  completion:nil];
```

######Check out the demo project for more details.

## License
SCStackViewController is released under the MIT License (MIT) (see the LICENSE file)

## Contact
Any suggestions or improvements are more than welcome.
Feel free to contact me at [stefan.ceriu@yahoo.com](mailto:stefan.ceriu@yahoo.com) or [@stefanceriu](https://twitter.com/stefanceriu).