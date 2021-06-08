# Layout redraw 



#### layoutIfNeeded()

Use this method to force the view to update its layout immediately. When using Auto Layout, the layout engine updates the position of views as needed to satisfy changes in constraints



#### setNeedsLayout()

Invalidates the current layout of the receiver and triggers a layout update during the next update cycle.

Call this method on your application’s main thread when you want to adjust the layout of a view’s subviews. This method makes a note of the request and returns immediately. Because this method does not force an immediate update, but instead waits for the next update cycle, you can use it to invalidate the layout of multiple views before any of those views are updated. This behavior allows you to consolidate all of your layout updates to one update cycle, which is usually better for performance.





# updateConstraints()

Override this method to optimize changes to your constraints.

To schedule a change, call [`setNeedsUpdateConstraints()`](https://developer.apple.com/documentation/uikit/uiview/1622450-setneedsupdateconstraints) on the view. The system then calls your implementation of `updateConstraints()` before the layout occurs. This lets you verify that all necessary constraints for your content are in place at a time when your custom view’s properties are not changing.

Your implementation must be as efficient as possible. Do not deactivate all your constraints, then reactivate the ones you need. Instead, your app must have some way of tracking your constraints, and validating them during each update pass. Only change items that need to be changed. During each update pass, you must ensure that you have the appropriate constraints for the app’s current state.



# setNeedsDisplay()

You can use this method or the [`setNeedsDisplay(_:)`](https://developer.apple.com/documentation/uikit/uiview/1622587-setneedsdisplay) to notify the system that your view’s contents need to be redrawn. This method makes a note of the request and returns immediately. The view is not actually redrawn until the next drawing cycle, at which point all invalidated views are updated.



You should use this method to request that a view be redrawn only when the content or appearance of the view change. If you simply change the geometry of the view, the view is typically not redrawn. Instead, its existing content is adjusted based on the value in the view’s [`contentMode`](https://developer.apple.com/documentation/uikit/uiview/1622619-contentmode) property. Redisplaying the existing content improves performance by avoiding the need to redraw content that has not changed