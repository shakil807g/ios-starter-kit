# UIPageViewController

`UIPageViewController` consists of a main content page, followed by a series of `UIViewController`s you stick inside it.

![demo](https://github.com/jrasmusson/ios-starter-kit/blob/master/basics/UIPageViewController/images/demo.gif)

```swift
@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        window = UIWindow(frame: UIScreen.main.bounds)
        window?.makeKeyAndVisible()

        window?.rootViewController = PageViewController(transitionStyle: .scroll, navigationOrientation: .horizontal, options: nil)

        return true
    }
```

```swift
//
//  PageViewController.swift
//  UIPageViewContoller
//
//  Created by Jonathan Rasmusson (Contractor) on 2018-09-13.
//  Copyright © 2018 Jonathan Rasmusson (Contractor). All rights reserved.
//

import Foundation
import UIKit

class PageViewController: UIPageViewController, UIPageViewControllerDataSource, UIPageViewControllerDelegate {

    var pages = [UIViewController]()
    let pageControl = UIPageControl()

    override func viewDidLoad() {
        super.viewDidLoad()

        dataSource = self
        delegate = self
        let initialPage = 0
        let page1 = ViewController1()
        let page2 = ViewController2()
        let page3 = ViewController3()

        // add the individual viewControllers to the pageViewController
        pages.append(page1)
        pages.append(page2)
        pages.append(page3)
        setViewControllers([pages[initialPage]], direction: .forward, animated: true, completion: nil)

        // pageControl
        pageControl.frame = CGRect()
        pageControl.currentPageIndicatorTintColor = UIColor.black
        pageControl.pageIndicatorTintColor = UIColor.lightGray
        pageControl.numberOfPages = pages.count
        pageControl.currentPage = initialPage
        view.addSubview(pageControl)

        pageControl.translatesAutoresizingMaskIntoConstraints = false
        pageControl.bottomAnchor.constraint(equalTo: view.bottomAnchor, constant: -5).isActive = true
        pageControl.widthAnchor.constraint(equalTo: view.widthAnchor, constant: -20).isActive = true
        pageControl.heightAnchor.constraint(equalToConstant: 20).isActive = true
        pageControl.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
    }

    func pageViewController(_ pageViewController: UIPageViewController, viewControllerBefore viewController: UIViewController) -> UIViewController? {

        if let viewControllerIndex = pages.index(of: viewController) {
            if viewControllerIndex == 0 {
                // wrap to last page in array
                return pages.last
            } else {
                // go to previous page in array
                return pages[viewControllerIndex - 1]
            }
        }
        return nil
    }

    func pageViewController(_ pageViewController: UIPageViewController, viewControllerAfter viewController: UIViewController) -> UIViewController? {

        if let viewControllerIndex = pages.index(of: viewController) {
            if viewControllerIndex < pages.count - 1 {
                // go to next page in array
                return pages[viewControllerIndex + 1]
            } else {
                // wrap to first page in array
                return pages.first
            }
        }
        return nil
    }

    func pageViewController(_ pageViewController: UIPageViewController, didFinishAnimating finished: Bool, previousViewControllers: [UIViewController], transitionCompleted completed: Bool) {

        // set the pageControl.currentPage to the index of the current viewController in pages
        if let viewControllers = pageViewController.viewControllers {
            if let viewControllerIndex = pages.index(of: viewControllers[0]) {
                pageControl.currentPage = viewControllerIndex
            }
        }
    }
}
```

```swift
//
//  ViewController.swift
//  UIPageViewContoller
//
//  Created by Jonathan Rasmusson (Contractor) on 2018-09-13.
//  Copyright © 2018 Jonathan Rasmusson (Contractor). All rights reserved.
//

import UIKit

class ViewController1: UIViewController {

    let billImageView: UIImageView = {
        let imageView = UIImageView()
        imageView.translatesAutoresizingMaskIntoConstraints = false
        imageView.backgroundColor = .red

        return imageView
    }()

    let titleLabel: UILabel = {
        let label = UILabel()
        label.translatesAutoresizingMaskIntoConstraints = false
        label.text = "Title1"
        label.font = UIFont.boldSystemFont(ofSize: 28)
        label.textAlignment = .center
        label.textColor = UIColor(red:0.2, green:0.26, blue:0.31, alpha:1)

        return label
    }()

    let bodyLabel: UILabel = {
        let label = UILabel()
        label.translatesAutoresizingMaskIntoConstraints = false
        label.text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec sit amet placerat elit. Fusce dictum arcu in velit rutrum maximus at nec sapien. Maecenas a enim nisl."
        label.numberOfLines = 0
        label.lineBreakMode = .byWordWrapping
        label.font = UIFont.systemFont(ofSize: 14)
        label.textColor = UIColor.gray

        return label
    }()

    override func viewDidLoad() {
        super.viewDidLoad()

        setupViews()
        view.backgroundColor = UIColor.white
    }

    func setupViews() {
        view.addSubview(billImageView)

        let containerStack = makeContainerStackView()
        containerStack.addArrangedSubview(titleLabel)
        containerStack.addArrangedSubview(bodyLabel)

        view.addSubview(containerStack)

        if #available(iOS 11, *) {
            billImageView.topAnchor.constraint(equalTo: view.safeAreaLayoutGuide.topAnchor, constant: 60).isActive = true
        } else {
            billImageView.topAnchor.constraint(equalTo: view.topAnchor, constant: 60).isActive = true
        }

        billImageView.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        billImageView.heightAnchor.constraint(equalToConstant: 250).isActive = true
        billImageView.widthAnchor.constraint(equalToConstant: 250).isActive = true

        containerStack.topAnchor.constraint(equalTo: billImageView.bottomAnchor, constant: 24).isActive = true
        containerStack.leadingAnchor.constraint(equalTo: view.leadingAnchor, constant: 24).isActive = true
        containerStack.trailingAnchor.constraint(equalTo: view.trailingAnchor, constant: -24).isActive = true
    }

    func makeLabel() -> UILabel {
        let label = UILabel()
        label.translatesAutoresizingMaskIntoConstraints = false
        label.numberOfLines = 0
        label.lineBreakMode = .byWordWrapping
        label.font = UIFont.systemFont(ofSize: 13)
        label.textColor = UIColor(red: 0.4, green: 0.4, blue: 0.4, alpha: 1)

        return label
    }

    func makeContainerStackView() -> UIStackView {
        let stack = UIStackView()
        stack.translatesAutoresizingMaskIntoConstraints = false
        stack.axis = .vertical
        stack.distribution = .fill
        stack.alignment = .fill
        stack.spacing = 16.0

        return stack
    }

}
```

```swift
//
//  ViewController.swift
//  UIPageViewContoller
//
//  Created by Jonathan Rasmusson (Contractor) on 2018-09-13.
//  Copyright © 2018 Jonathan Rasmusson (Contractor). All rights reserved.
//

import UIKit

class ViewController2: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        self.view.backgroundColor = UIColor.red

        let labelInst = UILabel()
        self.view.addSubview(labelInst)
        labelInst.text = "Page 2"
        labelInst.translatesAutoresizingMaskIntoConstraints = false
        labelInst.topAnchor.constraint(equalTo: self.view.topAnchor, constant: 50).isActive = true
        labelInst.leftAnchor.constraint(equalTo: self.view.leftAnchor, constant: 20).isActive = true
    }

}
```

## . TransitionStyle

There are two types of transitions files. `.pageCurl` was you saw at the top. The other is `.scroll`.

```swift
window?.rootViewController = PageViewController(transitionStyle: .scroll, navigationOrientation: .horizontal, options: nil)
```

![demo](https://github.com/jrasmusson/ios-starter-kit/blob/master/basics/UIPageViewController/images/scroll.gif)

## Adding animations

![demo](https://github.com/jrasmusson/ios-starter-kit/blob/master/basics/UIPageViewController/images/animated.gif)

To add animations to `UIPageViewController`s there are three things you need to look up for

- `UIViewController` lifecycle
- setting and reusing constraints
- manually calling for autolayout needs layout

Basically you need to break your setting up of constraints into three stages

- static
- preAnimate
- animate

Otherwise you will get conflicting constraints and the animation and static constraints try to clobber each other.

Secondly, for constraints you want to animate, you want to create them once and then reuse them. Simply change the constant to the pre and post animate values and let the animation do it's thing.

Thridly, and this is big, you need to call `layoutIfNeeded` at the right time to get the animation to render on top of the static views correctly. This one is confusing because without `UIPageViewController` you may not need to do this. But with it you do.

```swift
//
//  ViewController.swift
//  UIPageViewContoller
//
//  Created by Jonathan Rasmusson (Contractor) on 2018-09-13.
//  Copyright © 2018 Jonathan Rasmusson (Contractor). All rights reserved.
//

import UIKit

class ViewController1: UIViewController {

    var titleLabelTopConstraint : NSLayoutConstraint? // 1
    var bodyLabelTopConstraint : NSLayoutConstraint?

    let billImageView: UIImageView = {
        let imageView = UIImageView()
        imageView.translatesAutoresizingMaskIntoConstraints = false
        imageView.backgroundColor = .red

        return imageView
    }()

    let titleLabel: UILabel = {
        let label = UILabel()
        label.translatesAutoresizingMaskIntoConstraints = false
        label.text = "Title1"
        label.font = UIFont.boldSystemFont(ofSize: 28)
        label.textAlignment = .center
        label.textColor = UIColor(red:0.2, green:0.26, blue:0.31, alpha:1)

        return label
    }()

    let bodyLabel: UILabel = {
        let label = UILabel()
        label.translatesAutoresizingMaskIntoConstraints = false
        label.text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec sit amet placerat elit. Fusce dictum arcu in velit rutrum maximus at nec sapien. Maecenas a enim nisl."
        label.numberOfLines = 0
        label.lineBreakMode = .byWordWrapping
        label.font = UIFont.systemFont(ofSize: 14)
        label.textColor = UIColor.gray
        label.textAlignment = .center

        return label
    }()

    override func viewDidLoad() {
        super.viewDidLoad()

        setupStaticConstraints()
    }

    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)

        prepForAnimation()
    }

    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)

        animate()
    }

    func setupStaticConstraints() {
        view.backgroundColor = .white

        view.addSubview(billImageView)
        view.addSubview(titleLabel)
        view.addSubview(bodyLabel)

        billImageView.topAnchor.constraint(equalTo: view.safeAreaLayoutGuide.topAnchor, constant: 80).isActive = true
        billImageView.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        billImageView.heightAnchor.constraint(equalToConstant: 250).isActive = true
        billImageView.widthAnchor.constraint(equalToConstant: 250).isActive = true

        titleLabelTopConstraint = titleLabel.topAnchor.constraint(equalTo: billImageView.bottomAnchor, constant: 120) // 2
        titleLabelTopConstraint?.isActive = true
        titleLabel.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true

        bodyLabelTopConstraint = bodyLabel.topAnchor.constraint(equalTo: billImageView.bottomAnchor, constant: 120)
        bodyLabelTopConstraint?.isActive = true
        bodyLabel.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        bodyLabel.heightAnchor.constraint(equalToConstant: 250).isActive = true
        bodyLabel.widthAnchor.constraint(equalToConstant: 250).isActive = true

    }

    func prepForAnimation() {
        self.titleLabelTopConstraint?.constant = 60
        titleLabel.alpha = 0.0

        self.bodyLabelTopConstraint?.constant = 80
        bodyLabel.alpha = 0.0
    }

    func animate() {

        view.layoutIfNeeded() // forces pending layouts to complete before starting this animation

        UIView.animate(withDuration: 3) {
            self.titleLabelTopConstraint?.constant = 20 // 3
            self.titleLabel.alpha = 1
            self.view.layoutIfNeeded()
        }

        UIView.animate(withDuration: 3) {
            self.bodyLabelTopConstraint?.constant = -20
            self.bodyLabel.alpha = 1
            self.view.layoutIfNeeded()
        }
    }
}
```

