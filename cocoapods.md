# Cocoa Pods

```bash
pod init
```

```bash
open -a Xcode Podfile
```



Example pod file, update into something like this.

```
# Uncomment the next line to define a global platform for your project
platform :ios, '9.0'

target 'IMDBViewer' do
  # Comment the next line if you don't want to use dynamic frameworks
  use_frameworks!
  pod 'youtube-ios-player-helper', '~> 0.1.4'
  
  # Pods for IMDBViewer
  target 'IMDBViewerTests' do
    inherit! :search_paths
    # Pods for testing
  end

  target 'IMDBViewerUITests' do
    # Pods for testing
  end

end
```



```bash
pod install
```



Then close xcode and open the workspace file.



good to go!



