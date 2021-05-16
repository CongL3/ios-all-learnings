# Images

```
Image("natgeologo")
	.resizable()
	.aspectRatio(contentMode: .fill)
	.frame(width: 100, height: 90)
	.padding(.leading, 20)
	.padding(.top, UIApplication.shared.windows.first?.safeAreaInsets.top)
```



```
						Button(action: {
							withAnimation(.spring()){show.toggle()}
						}, label: {
							Image(systemName: "chevron.left")
								.foregroundColor(.black)
								.padding()
								.background(Color.white)
								.clipShape(Circle())
						})
```

