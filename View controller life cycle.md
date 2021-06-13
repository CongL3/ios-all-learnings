# View Controller Lide Cycle

#### loadView() 

- This is the method that creates the view for the view controller. You override this method only in case you want to build the whole interface for the view controller from code. Don’t use this unless there is a valid reason.
- If you are working with storyboards or nib files you do not have to do anything with this method and you can ignore it. Its implementation in UIVIewController loads the interface from the interface file and connects all the outlets and actions for you.



#### viewDidLoad()

- When this method gets called, the view of the view controller has been created and you are sure that all the outlets are in place.
- It is common to use this method to populate the user interface of the view controller with data before the user sees it.
- This method is called only once in the lifetime of a view controller, so you use it for things that need to happen only once. If you need to perform some task every time a view controller comes on screen, then you need the following methods.



#### viewWillAppear(_:)

- You override this method for tasks that you need to repeat every time a view controller comes on screen. This method can be called multiple times for the same instance of a view controller.



#### viewWillLayoutSubViews:

- Called to notify the view controller that its view is about to layout its subviews.
- This method is called every time the frame changes like for example when rotate or it’s marked as needing layout. It’s the first step where the view bounds are final.



#### viewDidLayoutSubviews:

 

#### viewDidAppear(_:)

- This method gets called after the view controller appears on screen.
- You can use it to start animations in the user interface, to start playing a video or a sound, or to start collecting data from the network.



#### viewWillDisappear(_:)

- Before the transition to the next view controller happens and the origin view controller gets removed from screen, this method gets called.



#### viewDidDisappear(_:)

- After a view controller gets removed from the screen, this method gets called.
- You usually override this method to stop tasks that are should not run while a view controller is not on screen.
- For example, you can stop listening to notifications, observing other objects properties, monitoring the device sensors or a network call that is not needed anymore.

 

 