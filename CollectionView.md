```swift
		let layout = UICollectionViewFlowLayout.init()
		layout.sectionInset = UIEdgeInsets(top: 20, left: 0, bottom: 10, right: 0)
		layout.estimatedItemSize = UICollectionViewFlowLayout.automaticSize
		layout.scrollDirection = .vertical
		layout.headerReferenceSize = CGSize.init(width: UIScreen.main.bounds.width, height: 40)
		layout.footerReferenceSize = CGSize.init(width: UIScreen.main.bounds.width, height: 0)
		collectionView.collectionViewLayout = layout

```

