**Demystifying SwiftUI**

[**https://developer.apple.com/videos/play/wwdc2021/10022/?utm_source=swiftlee&utm_medium=swiftlee_weekly&utm_campaign=issue_68**](https://developer.apple.com/videos/play/wwdc2021/10022/?utm_source=swiftlee&utm_medium=swiftlee_weekly&utm_campaign=issue_68)

Declarative UI Framework

​	Describe what you want at a high-level and SwiftUI decides how it’s displayed



SwiftUI is value types so there’s no pointers, all structs.



**Identity** 

ViewIdentity = Same identity then they transition in a different way

​	Explicit identity

​		Setting names -> name of different dogs

​		But someone has to keep track of all the names

​		SwiftUI uses other forms of explicit identity 

​		if the collection changes swift can use the identifiers to do the correct animations

​		We can set id to only the views we need to call out explicitly otherwise we don’t need to give them identifiers.

​		Every view has an identity even if its not explicit 

​	Structural identity

​		Swift UI uses structure to generate implicit identities for our views

​			By using the relative arrangement of the subjects

​			If statements 

​			This allows swift to give it a implicit stable identity behind the scenes 

​			Type + Position

SwiftUI prefers an approach where we preserve identity and provide more fluid transitions



AnyView! The enemy -> Type erasing wrapper type -> Hides the type of the view

​	Functions can only return 1 view so we use the any view wrapper 

​	Avoid any view when ever possible 15:44



<img src="/Users/congle/Documents/Dev/ios-all-learnings/Images/image-20210623204716540.png" alt="image-20210623204716540" style="zoom:50%;" />

**Lifetime**

Identities allow us to give continuity over time



When the ID changes the state is replaced.

State lifetime = view lifetime 



**<img src="/Users/congle/Documents/Dev/ios-all-learnings/Images/image-20210623204835491.png" alt="image-20210623204835491" style="zoom:50%;" />**





**Dependencies**



These 3 are used to determine what we see in SwiftUI

 -> Data Essentials in SwiftUI WWDC2020





<img src="/Users/congle/Documents/Dev/ios-all-learnings/Images/image-20210623204937010.png" alt="Pasted Graphic 5.png" style="zoom:50%;" />



<img src="/Users/congle/Documents/Dev/ios-all-learnings/Images/image-20210623204954013.png" alt="image-20210623204954013" style="zoom:50%;" />



<img src="/Users/congle/Library/Application Support/typora-user-images/image-20210623205101290.png" alt="image-20210623205101290" style="zoom:50%;" />

Moving conditions inside the modifier improves performance

![Pasted Graphic 11.png](blob:file:///9db9460d-acc7-4fd0-9bcf-03190cca6980)



@ViewBuilder

Allows us to remove the any view wrapper and return statements 

<img src="/Users/congle/Documents/Dev/ios-all-learnings/Images/image-20210623205133342.png" alt="image-20210623205133342" style="zoom:50%;" />

<img src="/Users/congle/Documents/Dev/ios-all-learnings/Images/image-20210623205147878.png" alt="image-20210623205147878" style="zoom:50%;" />

<img src="/Users/congle/Documents/Dev/ios-all-learnings/Images/image-20210623205209749.png" alt="image-20210623205209749" style="zoom:50%;" />



<img src="/Users/congle/Documents/Dev/ios-all-learnings/Images/image-20210623205227724.png" alt="image-20210623205227724" style="zoom:50%;" />