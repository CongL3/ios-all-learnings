## Overview 

```swift
NSManagedObjectContext -> Scrap pad
// Changes to managed objects remain in memory in the associated context until Core Data saves that context to one or more persistent stores
```

```Swift
NSPersistentStoreCoordinator -> What you actually put away
* Coordinators do the opposite of providing for concurrency—they serialize operations.
* Note that if multiple threads work directly with a coordinator, they need to lock and unlock it explicitly
```

MOC -> can have parent MOCs that deal with fetch and save

This pattern has a number of usage scenarios, including:

* Performing background operations on a second thread or queue.
* Managing discardable edits, such as in an inspector window or view



```swift
NotificationCenter.default.addObserver(self, 
                                       selector: #selector(<#methodToCall#>),                                     
                                       name: .NSManagedObjectContextDidSave,                                       
                                       object: <#managedObjectContext#>)
```

```swift
ObservableObject
// A type of object with a publisher that emits before the object has changed.
// By default an ObservableObject synthesizes an objectWillChange publisher that emits the changed value before any of its @Published properties changes.
//Seems similar to KVO


```



## Concurrency



## Core data with NSFetchResultsController

https://github.com/CongL3/everyLife/commit/825c412e296bc5e9eeb608fe5d2c509644e2e83a

Minimal code for just 1 custom item with core data





## Core data with @FetchRequest

Inside the @main you have to include 

```swift
@main
struct FavouriteMovies_CoreDataApp: App {
    let persistenceController = PersistenceController.shared

    var body: some Scene {
        WindowGroup {
			// passes managed object context to all child views
      
            ContentView()
                .environment(\.managedObjectContext, persistenceController.container.viewContext)
        }
    }
}
```



Inside of `ContentView`, you can create a property that's annotated with `@FetchRequest` to fetch objects using the managed object context that you injected into `MainView`'s environment



```swift
struct MainView: View {
  @FetchRequest(
    // Entity  use .entity().
    entity: TodoItem.entity(),
		// Empty array can be passed if sorting is not required.
    sortDescriptors: [NSSortDescriptor(key: "dueDate", ascending: true)]
  ) var items: FetchedResults<TodoItem>

  var body: some View {
    List(items) { item in
      Text(item.name)
    }
  }
}
```



`@FetchRequest`  will automatically refresh your view if any of the fetched objects are updated



```swift
@FetchRequest(
  entity: TodoItem.entity(),
  sortDescriptors: [NSSortDescriptor(key: "dueDate", ascending: true)],
  // Use of predicate here, nextWeek() is a custom value
  predicate: NSPredicate(format: "dueDate < %@", Date.nextWeek() as CVarArg)
) var tasksDueSoon: FetchedResults<TodoItem>
```





```swift
// a convenient extension to set up the fetch request
extension TodoItem {
  static var dueSoonFetchRequest: NSFetchRequest<TodoItem> {
    let request: NSFetchRequest<TodoItem> = TodoItem.fetchRequest()
    request.predicate = NSPredicate(format: "dueDate < %@", Date.nextWeek() as CVarArg)
    request.sortDescriptors = [NSSortDescriptor(key: "dueDate", ascending: true)]

    return request
  }
}

// in your view
// very nice to create a custom fetch request dueSoonFetchRequest
@FetchRequest(fetchRequest: TodoItem.dueSoonFetchRequest)
var tasksDueSoon: FetchedResults<TodoItem>
```





References: 
https://www.donnywals.com/fetching-objects-from-core-data-in-a-swiftui-project/

