## Collection Views: how to add a header / title that wraps

This is _not_ as straight forward as I would hope!

First, create a header view, which is of type `UICollectionReusableView`:

```
final class CollectionHeaderView: UICollectionReusableView {
    
    private let titleLabel = UILabel.wrapText() // this is a custom helper to set lines to 0 and wrap the text

    override init(frame: CGRect) {
        super.init(frame: frame)
        addConstrainedSubview(titleLabel)
    }
    
    required init?(coder: NSCoder) { ibNotSupported() }
    
    func set(title: String) {
        titleLabel.text = title
    }
}
```

Then in the view controller:

```
extension PerksAvailableViewController: UICollectionViewDataSource {

    func collectionView(_ collectionView: UICollectionView, viewForSupplementaryElementOfKind kind: String, at indexPath: IndexPath) -> UICollectionReusableView {
        if kind == UICollectionView.elementKindSectionHeader {
            let headerView = collectionView.dequeueReusableSupplementaryView(ofKind: kind, withReuseIdentifier: "CollectionHeaderView", for: indexPath) as! CollectionHeaderView
            headerView.set(title: viewModel.title)
            return headerView
        } else {
            fatalError() // TODO: make better
        }
    }
}

extension PerksAvailableViewController: UICollectionViewDelegateFlowLayout {

    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, referenceSizeForHeaderInSection section: Int) -> CGSize {
        
        if #available(iOS 11.0, *) {
            let indexPath = IndexPath(row: 0, section: section)
            let headerView = self.collectionView(collectionView, viewForSupplementaryElementOfKind: UICollectionView.elementKindSectionHeader, at: indexPath)
            
            return headerView.systemLayoutSizeFitting(CGSize(width: collectionView.frame.width, height: UIView.layoutFittingExpandedSize.height),
                                                      withHorizontalFittingPriority: .required,  // keep the width the same
                                                      verticalFittingPriority: .fittingSizeLevel) // resize the height
        } else {
            return CGSize(width: collectionView.frame.width, height: 32.0)
        }
    }
}
```

Don't forget to register the header view:
`view.register(CollectionHeaderView.self, forSupplementaryViewOfKind: UICollectionView.elementKindSectionHeader, withReuseIdentifier: "CollectionHeaderView")`



