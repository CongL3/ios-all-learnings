## Core data with abstraction





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

