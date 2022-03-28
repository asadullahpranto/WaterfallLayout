# WaterfallLayout
A waterfall-like (Pinterest-style) layout for UICollectionView.

This library is a modified lightweight Swift version based on [`CHTCollectionViewWaterfallLayout`](https://github.com/chiahsien/CHTCollectionViewWaterfallLayout.git), very easy to integrated with your existing project just like using Apple official `UICollectionViewFlowLayout` and `UICollectionViewDelegateFlowLayout`.

## Preview
![Preview](preview.gif)

## Requirements
iOS 9.0+

## Installation
#### Swift Package Manager (Recommended)

- Xcode >  File > Swift Packages > Add Package Dependency
- Add `https://github.com/Jinya/WaterfallLayout.git`
- Select "Exact Version" (recommend using the latest exact version)

## Package content
Only contains:

- `UICollectionViewWaterfallLayout.swift`
- `UICollectionViewDelegateWaterfallLayout.swift`

and a mini example project.

## Example codes
```swift
import UIKit
import WaterfallLayout

// collection view cell
class WaterfallViewCell: UICollectionViewCell {
    static let reuseIdentifier = "reuseIdentifier"

    let titleLabel = UILabel()

    override init(frame: CGRect) {
        super.init(frame: frame)

        titleLabel.font = .preferredFont(forTextStyle: .title1)
        titleLabel.textColor = .white
        titleLabel.textAlignment = .center
        titleLabel.layer.cornerRadius = 12
        titleLabel.clipsToBounds = true

        contentView.addSubview(titleLabel)
        titleLabel.translatesAutoresizingMaskIntoConstraints = false
        NSLayoutConstraint.activate([
            titleLabel.topAnchor.constraint(equalTo: contentView.topAnchor),
            titleLabel.leftAnchor.constraint(equalTo: contentView.leftAnchor),
            titleLabel.rightAnchor.constraint(equalTo: contentView.rightAnchor),
            titleLabel.bottomAnchor.constraint(equalTo: contentView.bottomAnchor)
        ])
    }

    required init?(coder: NSCoder) { fatalError() }
}

// collection view controller
class ViewController: UIViewController {

    var waterfallView: UICollectionView!

    override func viewDidLoad() {
        super.viewDidLoad()

        let layout = UICollectionViewWaterfallLayout()
        layout.columnCount = 2
        layout.minimumColumnSpacing = 20
        layout.minimumInteritemSpacing = 20
        layout.sectionInset = UIEdgeInsets(top: 10, left: 10, bottom: 10, right: 10)

        waterfallView = UICollectionView(frame: .zero, collectionViewLayout: layout)
        waterfallView.register(WaterfallViewCell.self, forCellWithReuseIdentifier: WaterfallViewCell.reuseIdentifier)
        waterfallView.dataSource = self
        waterfallView.delegate = self

        view.addSubview(waterfallView)
        waterfallView.translatesAutoresizingMaskIntoConstraints = false
        NSLayoutConstraint.activate([
            waterfallView.topAnchor.constraint(equalTo: view.topAnchor),
            waterfallView.leftAnchor.constraint(equalTo: view.leftAnchor),
            waterfallView.rightAnchor.constraint(equalTo: view.rightAnchor),
            waterfallView.bottomAnchor.constraint(equalTo: view.bottomAnchor)
        ])
    }
}

// collection view data source
extension ViewController: UICollectionViewDataSource {
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return 100
    }

    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        guard let cell = collectionView.dequeueReusableCell(withReuseIdentifier: WaterfallViewCell.reuseIdentifier, for: indexPath) as? WaterfallViewCell else {
            fatalError()
        }
        let titles: [String] = ["Shanghai", "Chongqing", "New York", "San Francisco", "Tokyo", "Phuket", "Singapore", "Wuhan", "Shenzhen", "Los Angeles"]
        let colors: [UIColor] = [.red, .magenta, .brown, .blue, .purple, .blue, .cyan, .gray, .green, .yellow, .purple]
        cell.titleLabel.text = titles.randomElement()!
        cell.titleLabel.backgroundColor = colors.randomElement()!
        return cell
    }
}

// collection view delegate and waterfall layout delegate
extension ViewController: UICollectionViewDelegateWaterfallLayout {
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
        let randomHeights: [CGFloat] = [300, 400, 500, 600, 700, 800]
        return CGSize(width: 500, height: randomHeights.randomElement()!)
    }
    
    func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
        print("did select item at \(indexPath.item)")
    }
}

```

## MIT License 

WaterfallLayout released under the MIT license. See LICENSE for details.

## Acknowledgement

[`CHTCollectionViewWaterfallLayout`](https://github.com/chiahsien/CHTCollectionViewWaterfallLayout.git)
