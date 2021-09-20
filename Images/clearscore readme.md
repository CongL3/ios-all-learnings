\# Clearscore Test



\## Notes

Have a few todos and could dos at the bottom. If ya want i'd be happy to implement them too.





Used MVVM - C / Nibs / ChildVCs

Created using XCode Version 12.5 (12E262) and using Montery 12.0 Beta (21A5304g)





\## UITests

No mocking for UITests

To mock for UITests for a take home assignment I've used Swifter (https://github.com/httpswift/swifter)

and that worked well for a simple project to prove out the concept and usage.



\## IBDesignable

I ran into a issue where they weren't showing up.



![Screenshot](ReadMeImages/IBDesignableIssue.png)



The path it was searching for doesnt exist. I had to change the folder "Debug-iphonesimulator" to "Debug-iphoneos" to get them the IBInspectable properties to show up. (this had to be done after building and then closing and opening xcode to refresh the view)



Not the most ideal situation. Would need to investigate further to find the cause/appropriate but for this excercise I thought it was appropriate to leave as it.



\## Not implemented / Todos

\* Networking layer - currently a hard coded url. Could use a stack to build the URL and any passed values. 



Custom network errors

Theres some null values from the server. 



Unit tests for networking - I would try to mock it all out. Gave it a start and realised it was quite a tall order for this exercise.



Localised strings

Accessibility



Details screen

Strings to come from VMs in loading and error views