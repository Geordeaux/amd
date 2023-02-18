---
id: snapshot-testing-technical-guide
title: Snapshot Testing Technical Guide
---

----

**iOS Snapshot testing technical guide**

| Author           | mike-cap                                                     |
| ---------------- | ------------------------------------------------------------ |
| Status           | WIP                                                          |
| **OKR document** | [link](https://docs.google.com/document/d/1S9a4Oh9_m3taXimf_sCnEyrx5Ypia_EnG3Jxioq0yFI/edit#heading=h.tsgq3yo6wsfj) |
| Epic             | [**[APP-2432](https://jira.toolsfdg.net/browse/APP-2432?src=confmacro)**] |
| Version             | 1.0 |

----

# Purpose of this document

This project is part of the OKR (**KR4 Snapshot tests**) for Q1 2021. 

As the time of writing (Jan 15, 2021), Binance iOS app (from here on will be referred to as **APP** for short) hasn't had any Unit Tests (UTs) and code coverage is very low.

As an effort to improve APP's stability and quality, engineers have make some plans to provide Unit Tests to increase code coverage and prevent regression issues, one of which is to add snapshot test cases for existing UI classes (especially for **BNCUIKit**). 

# A glance at snapshot tests. 

When we add a custom UI element (UIView/UIViewController) to our app, we will want to make sure that the elements will always work as intended and there will be no unexpected changes when we modify the code. **Snapshot testing** is a very easy and effective solution to serve the purpose. 

As its name implies, **Snapshot Test** works by comparing a latest snapshot of our custom UI element with a previously-taken referenced image. If the two images are identical (by default, it's pixel-by-pixel comparison - However we can set a tolerance for the comparison if need be) the test will pass, if they are different the test will fail and we will know that something has changed on the UI.

![snapshot-testing-visualization](https://static.devfdg.net/static/mono-static/docs-ui/img/image2021-1-18_12-46-52.png)

**Important note**: We should not confuse snapshot tests with **UI tests**. Snapshot Tests are can be simply considered **Unit Tests** for UI elements. It does not require a [XCUIApplication](https://developer.apple.com/documentation/xctest/xcuiapplication) nor does it support app event simulations (keypress/button taps, etc..). 

# Adding snapshot tests to Binance project. 

## 1. Tool

To write and run snapshot tests in our project, we will use **iOSSnapshotTestCase**, a popular 3rd-party framework developed by Uber (previously by Facebook). This framework is available via Cocoapods, it provides convenient APIs for snapshot testing UIViews and UIViewControllers. More information can be found at https://github.com/uber/ios-snapshot-test-case

## 2. Process

With the current monorepo structure we're using for our project, we can easily add separate test code and test targets to keep the codebase clean and robust. 
Thanks to Cocoapods, we can add the test target and test codes into the **Example** project of each module by adding a **test spec** to the **podspec** file of defined by the Module. More information about adding test specs can be found [here](https://guides.cocoapods.org/using/test-specs.html).

Side note: A great advantage of adding tests case to the Example workspace is that when we are writing the tests, we only need to compile the module we're working on, thus speed up the development process. Whereas if we write and run test on the Binance workspace, it will take very long to compile the whole project. 

Throughout this chapter, I'll use **BNCUIKit** as an example of how we can setup and add the tests to a module. 

### a. Introducing BNCSnapshotTestUtils

![modules-BNCSnapshotTestUtils](https://static.devfdg.net/static/mono-static/docs-ui/img/image2021-1-18_13-46-49.png)

As mentioned above, we will be using **iOSSnapshotTestCase** as a tool to run snapshot tests and we will create a dedicated module named **BNCSnapshotTestUtils** to wrap the its APIs into our own helper classes/APIs so that we can easily leverage to write/run the tests. **BNCSnapshotTestUtils** is also where we place common helper codes for testings such as extensions and helper/utility functions. 

To leverage the existing monorepo structure and Cocapods, we will make **BNCSnapshotTestUtils** a local module inside **ios-client/Modules**, and add it as a dependency whenever needed by the modules that need to be snapshot-tested. 

### b. Adding Test target and **BNCSnapshotTestUtils as a dependency using Cocoapods**

If we want to write and run snapshot tests to a specific module, we'll need to add a **test target** into the **Example** workspace of the module, and add **BNCSnapshotTestUtils** as a dependency for the test target. This can easily be done using Cocoapods by adding a **test_spec** to the end of the **podspec** file of the module. For example, this is a snippet for **BNCUIKit**:

**BNCUIKit.podspec**

```python
Pod::Spec.new do |s|
    
...

  s.test_spec 'SnapshotTests' do |ts|
    ts.source_files = 'Example/BNCUIKit-Unit-SnapshotTests/**/*.swift' # Define the path where test codes are stored, it's best to keep a consistent format like <ModuleMame_SnapshotTests>//../*.swift
    ts.ios.dependency 'BNCSnapshotTestUtils'
  end
end
```

Because we just declared a new test specs, we will need to provide some sourcecode for Cocoapods to validate the spec. Go to your **Example** project folder, then create a file following this path `/Example/<Module name>-Unit-SnapshotTests/Tests.swift`. 

![test_placeholder_file](https://static.devfdg.net/static/mono-static/docs-ui/img/placeholder_file.png)

Next, we need to modify **Example's** **Podfile** so that Cocoapods can automatically generate the test target and hook it to our project. Open the **Podfile** in the **Example** folder and add those lines (*Remember to remove the comments*) :

```python
...
target 'BNCUIKit_Example' do
  pod 'BNCUIKit', :path => '../', testspecs: ['SnapshotTests'] # <- (1) Specify the test target from podspec
  ...
end

abstract_target 'TestTarget' do # <- (2) Because we're using monorepo, we need to add these lines so that Cocoapods knows where to look for BNCSnapshotTestUtils 
  mod_pod 'BNCSnapshotTestUtils'
end

```

Now go to your **Example** project and run `bundle exec pod install`, if everything goes well, the outcome will look like this. 

![pod_install_result](https://static.devfdg.net/static/mono-static/docs-ui/img/pod_install_result.jpg)

Open your `.xcworkspace` from your `Example` folder and follow the below screenshot, you should be able to see the `<Module name>-Unit-SnapshotTests` target, select the target and click "Add".

![test_target_added](https://static.devfdg.net/static/mono-static/docs-ui/img/test_target_added.jpg)

And in your project navigator pane, you should also see a **SnapshotTests** group created automatically by Cocoapods located in `Pods/Development Pods/<Module name>/SnapshotTests` that contains a `Tests.swift` that we created in the previous step.

![snapshot_group](https://static.devfdg.net/static/mono-static/docs-ui/img/snapshot_group.jpg)

Congratulations! You are now all set to add and run snapshots tests for your module 🎉 🥳 

### c. Add and run snapshot tests

After you've added test target, let's try to run the target by selecting simulator `iPhone 8 (iOS 14.x)` and press `CMD+U`.  The project should compile and all the tests (which current has 0 testcases) should pass. 

![first_test_succeeded](https://static.devfdg.net/static/mono-static/docs-ui/img/first_test_succeeded.jpg)

Now it's time to add your first test cases to the module. 

Remember the `Tests.swift` we created earlier? It has fulfilled its  purpose and can be put to rest, we can delete it now. 

Let's now create the actual test code, in this example I'll add tests for BNCUIKit's `BNCCheckbox` . Create a `BNCCheckBoxSnapshotTests.swift` from `Pods/Development Pods/<Module name>/SnapshotTests` 

![first_test_file](https://static.devfdg.net/static/mono-static/docs-ui/img/first_test_file.jpg)

**Note**: When adding a new test file, please make sure: 

1. The file name should be `<Class we want to test>SnapshotTests.swift` 
2. The path should be under the `<Module name>-Unit-SnapshotTests` folder we created earlier
3. The new file must be added to `<Module name>-Unit-SnapshotTests` target

Open the new test file and do as the following: 

```swift
import BNCSnapshotTestUtils // (1) import BNCSnapshotTestUtils 
@testable import BNCUIKit // (2) import the module we want to test

final class BNCCheckBoxSnapshotTests: BNCSnapshotTestCase { // (3) class name should be <Class we want to test>SnapshotTests.swift, and it should also be `final`
  
    open override func setUp() { // Override setUp() method from superclass
        super.setUp()
        recordMode = true  // Set recordMode = true, this will be explained shortly
    }
  
    func testSomeThing() { // (4) This is the actual test case we want to run, the func must start with `test`
        let view = BNCCheckbox(frame: CGRect.init(x: 0, y: 0, width: 100, height: 100)) // (5) Create the view we want to test
        verifyView(view) // Test the view!
    }
}
```

Run click on the play button to the left of `func testSomeThing()`,  your test will be executed and you should get this message from XCode:

---

*failed - Test ran in record mode. Reference image is now saved. Disable record mode to perform an actual snapshot comparison!*

---

As the message suggests: You just ran your test in `record mode`, which means that after your test is executed, it will generate a **reference image** that can be used as the reference for comparison in latter runs. By now you should be asking 2 questions:

1. Where is this reference image stored in my system? 
2. After I have this **reference image**, how do I perform actual comparison in latter runs? 

Let's answer them one by one in the following section

### 1. Configure your Example scheme to store the reference image(s).

According to the tutorial from [iOSSnapshotTestCase](https://github.com/uber/ios-snapshot-test-case) , we must provide a path for the tests to store the reference and diff images by defining a environment variable in your test scheme. Now, edit our **Example's** test scheme as followed:

![example_scheme_config](https://static.devfdg.net/static/mono-static/docs-ui/img/example_scheme_config.jpg)

Run the test again and now you will be able to see a **Snapshots** folder right under `<Module name>-UnitTest-SnapshotTests` . This is where the generated reference images are stored. You can now dive deeper into this folder and find the generated **reference image** from the test run (in our case the reference image will be name `testSomething_XXX`, with `XXX` being the name of the class under test). 

![snapshot_created](https://static.devfdg.net/static/mono-static/docs-ui/img/snapshot_created.jpg)

### 2. Enable snapshot comparison

According to the tutorial from [iOSSnapshotTestCase](https://github.com/uber/ios-snapshot-test-case), our tests will base on the value of `recordMode` to either:

- Generate a reference image
- Perform the actual comparison between the latest snapshot of our UI element and its reference image.

Our main test class, `BNCSnapshotTestCase` is actually a subclass of `FBSnapshotTestCase` , since we already generated the reference image from our previous test run, let's comment out `recordMode = true` and run the test again. Voila! You've successfully passed your first test case!

![actual_test_succeeded](https://static.devfdg.net/static/mono-static/docs-ui/img/actual_test_succeeded.jpg)

Now let's try to do something more interesting, open the source code of the class we're testing and modify the some code to change the UI (In this example I'll change the *backgroundColor* to *red* -  **remember to undo this before you commit your code!**), then run the test `testSomething()` again and see the outcome:

---

*failed - Snapshot comparison failed: ...*

---

Pretty much self-explanatory isn't it? You changed something in the code that affects the UI, and when your test runs, it takes a latest snapshot image of your view and compare that with the corresponding **reference image**, turned out those two images are not identical, and the test will fail. 

Now remember the **Snapshots**  folder from earlier? Try open that folder again and you'll see something there! 

![test_failed_diff](https://static.devfdg.net/static/mono-static/docs-ui/img/test_failed_diff.jpg)

This is where **iOSSnapshotTestCase** really shines: When there is as failed tests, it will generated 3 files for dianogstic purpose: 

- **reference_testSomething_XXX**: The reference image 
- **failed_testSomething_XXX**: The latest snapshot which failed the test
- **diff_testSomething_XXX**: Overlaps the reference image and failed image into one photo, so we can have a visual look of what went wrong with our view. 

**And that's pretty much it! Now you can add more snapshot test cases for your UI components!** 

