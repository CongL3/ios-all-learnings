```swift
//
//  NetworkService.swift
//  everyLife
//
//  Created by Cong Le on 14/05/2021.
//

import Foundation
import Network

class NetworkService : ObservableObject {

	static let shared = NetworkService()

	let monitor = NWPathMonitor()
	let queue = DispatchQueue(label: "Network")

	@Published var connected: Bool = false
	
	private init() {
		
		monitor.pathUpdateHandler = { path in
			
				switch path.status {
				case .satisfied:
					print("satisfied")
					OperationQueue.main.addOperation {
						self.connected = true
					}
				case .requiresConnection:
					print("requiresConnection")
					OperationQueue.main.addOperation {
						self.connected = false
					}
				case .unsatisfied:
					print("unsatisfied")
					OperationQueue.main.addOperation {
						self.connected = false
					}
				default:
					OperationQueue.main.addOperation {
						self.connected = false
					}
				}
			}
		monitor.start(queue: queue)
	}
}
```



![E31kC9XWQAUTQio](/Users/congle/Documents/Dev/ios-all-learnings/Images/E31kC9XWQAUTQio.png)

![E31kC9hXEAEkTWl](/Users/congle/Documents/Dev/ios-all-learnings/Images/E31kC9hXEAEkTWl.png)

