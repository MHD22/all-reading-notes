##### [back-home](https://mhd22.github.io/all-reading-notes/main-table)

# Read: 31 - Espresso

<hr>

# Espresso basics:

## API components

The main components of Espresso include the following:

* **Espresso** – Entry point to interactions with views (via onView() and onData()). Also exposes APIs that are not necessarily tied to any view, such as pressBack().
* **ViewMatchers** – A collection of objects that implement the Matcher<? super View> interface. You can pass one or more of these to the onView() method to locate a view within the current view hierarchy.
* **ViewActions** – A collection of ViewAction objects that can be passed to the ViewInteraction.perform() method, such as click().
* **ViewAssertions** – A collection of ViewAssertion objects that can be passed the ViewInteraction.check() method. Most of the time, you will use the matches assertion, which uses a View matcher to assert the state of the currently selected view.
Example:

```java
// withId(R.id.my_view) is a ViewMatcher
// click() is a ViewAction
// matches(isDisplayed()) is a ViewAssertion
onView(withId(R.id.my_view))
    .perform(click())
    .check(matches(isDisplayed()));
```

## Find a view

* Finding a view by its R.id is as simple as calling onView():

```java
onView(withId(R.id.my_view));
```

* Sometimes, R.id values are shared between multiple views. When this happens an attempt to use a particular R.id gives you an exception, such as `AmbiguousViewMatcherException`. The exception message provides you with a text representation of the current view hierarchy, which you can search for and find the views that match the non-unique R.id:

```java
java.lang.RuntimeException:
androidx.test.espresso.AmbiguousViewMatcherException
This matcher matches multiple views in the hierarchy: (withId: is <123456789>)

...

+----->SomeView{id=123456789, res-name=plus_one_standard_ann_button,
visibility=VISIBLE, width=523, height=48, has-focus=false, has-focusable=true,
window-focus=true, is-focused=false, is-focusable=false, enabled=true,
selected=false, is-layout-requested=false, text=,
root-is-layout-requested=false, x=0.0, y=625.0, child-count=1}
****MATCHES****
|
+------>OtherView{id=123456789, res-name=plus_one_standard_ann_button,
visibility=VISIBLE, width=523, height=48, has-focus=false, has-focusable=true,
window-focus=true, is-focused=false, is-focusable=true, enabled=true,
selected=false, is-layout-requested=false, text=Hello!,
root-is-layout-requested=false, x=0.0, y=0.0, child-count=1}
****MATCHES****
```

* Looking through the various attributes of the views, you may find uniquely identifiable properties. In the example above, one of the views has the text "Hello!". You can use this to narrow down your search by using combination matchers:

```java
onView(allOf(withId(R.id.my_view), withText("Hello!")));
```

* You can also choose `not` to reverse any of the matchers:

```java
onView(allOf(withId(R.id.my_view), not(withText("Unwanted"))));
```

***Considerations***

* If the target view is inside an AdapterView—such as `ListView`, `GridView`, or `Spinner`—the `onView()` method might not work. In these cases, you should use `onData()` instead.


## Perform an action on a view:

* You can execute more than one action with one perform call:

```java
onView(...).perform(typeText("Hello"), click());
```

* If the view you are working with is located inside a ScrollView (vertical or horizontal), consider preceding actions that require the view to be displayed—such as click() and typeText()—with `scrollTo()`. This ensures that the view is displayed before proceeding to the other action:

```java
onView(...).perform(scrollTo(), click());
```

>Note: The scrollTo() method will have no effect if the view is already displayed so you can safely use it in cases when the view is displayed due to larger screen size, such as when your tests run on both smaller and larger screen resolutions.

## Check view assertions:

***Assertions can be applied to the currently selected view with the check() method.***

* For example, to check that a view has the text "Hello!":

```java
onView(...).check(matches(withText("Hello!")));
```

* If you want to assert that "Hello!" is content of the view, the following is considered bad practice:

```java
// Don't use assertions like withText inside onView.
onView(allOf(withId(...), withText("Hello!"))).check(matches(isDisplayed()));
```

## Check data loading in adapter views

***Adapter view simple test***:

* **Open the item selection**

```java
onView(withId(R.id.spinner_simple)).perform(click());
```

* **Select an item**

```java
onData(allOf(is(instanceOf(String.class)), is("Americano"))).perform(click());
```

* **Verify text is correct**

```java
onView(withId(R.id.spinnertext_simple))
    .check(matches(withText(containsString("Americano"))));
```

## Debugging:

* Espresso provides useful debugging information when a test fails:
  * Espresso logs all view actions to logcat.
  * Espresso prints the view hierarchy in the exception message when `onView()` fails.
    * If `onView()` does not find the target view, a NoMatchingViewException is thrown. You can examine the view hierarchy in the exception string to analyze why the matcher did not match any views.
    * If `onView()` finds multiple views that match the given matcher, an AmbiguousViewMatcherException is thrown. The view hierarchy is printed and all views that were matched are marked with the MATCHES label


<hr>

# Espresso recipes:

## Match a view next to another view:

* Often, the non-unique view will be paired with some unique label that’s located next to it, such as a name of the contact next to the call button. In this case, you can use the hasSibling() matcher to narrow down your selection:

```java
onView(allOf(withText("7"), hasSibling(withText("item: 0"))))
    .perform(click());
```

## Assert that a view is not displayed

```java
onView(withId(R.id.bottom_left))
    .check(matches(not(isDisplayed())));
```

***The above approach works if the view is still part of the hierarchy. If it is not, you will get a NoMatchingViewException and you need to use `ViewAssertions.doesNotExist()`.***


<hr>

# Create UI tests with Espresso Test Recorder:

*The Espresso Test Recorder tool lets you create UI tests for your app without writing any test code. By recording a test scenario, you can record your interactions with a device and add assertions to verify UI elements in particular snapshots of your app. Espresso Test Recorder then takes the saved recording and automatically generates a corresponding UI test that you can run to test your app.*

## Record an Espresso test:

### Record UI interactions:

To start recording a test with Espresso Test Recorder, proceed as follows:

* Click `Run` > Record Espresso Test.
* In the Select Deployment Target window, choose the device on which you want to record the test.
* Espresso Test Recorder triggers a build of your project, and the app must install and launch before Espresso Test Recorder allows you to interact with it. The Record Your Test window appears after the app launches, and since you have not interacted with the device yet, the main panel reads "No events recorded yet." Interact with your device to start logging events such as "tap" and "type" actions.


### Add assertions to verify UI elements

Assertions verify the existence or contents of a View element through three main types:

* text is: Checks the text content of the selected View element
* exists: Checks that the View element is present in the current View hierarchy visible on the screen
* does not exist: Checks that the View element is not present in the current View hierarchy

***To add an assertion to your test, proceed as follows:***

1. Click Add Assertion. A Screen Capture dialog appears while Espresso gets the UI hierarchy and other information about the current app state. The dialog closes automatically once Espresso has captured the screenshot.
2. A layout of the current screen appears in a panel on the right of the Record Your Test window. To select a View element on which to create an assertion, click on the element in the screenshot or use the first drop-down menu in the Edit assertion box at the bottom of the window. The selected View object is highlighted in a red box.
3. Select the assertion you want to use from the second drop-down menu in the Edit assertion box. Espresso populates the menu with valid assertions for the selected View element.
   * If you choose the "text is" assertion, Espresso automatically inserts the text currently inside the selected View element. You can edit the text to match your desired assertion using the text field in the Edit assertion box.
4. Click Save and Add Another to create another assertion or click Save Assertion to close the assertion panels.

<!-- <img src="https://developer.android.com/studio/images/test/espresso-test-recorder-assertion_2-2_2x.png" /> -->


### Save a recording

* Once you finish interacting with your app and adding assertions, use the following steps to save your recording and generate the Espresso test:

1. Click Complete Recording. The Pick a test class name for your test window appears.
2. Espresso Test Recorder gives your test a unique name within its package based on the name of the launched activity. Use the Test class name text field if you want to change the suggested name. Click Save.
   * If you have not added the Espresso dependencies to your app, a Missing Espresso dependencies dialog appears when you try to save your test. Click Yes to automatically add the dependencies to your build.gradle file.
3. The file automatically opens after Espresso Test Recorder generates it, and Android Studio shows the test class as selected in the Project window of the IDE.
   * Where the test saves depends on the location of your instrumentation test root, as well as the package name of the launched activity. For example, tests for the Notes testing app save in the src > androidTest > java > com.example.username.appname folder of the app module on which you recorded the test.



<hr>

### This file wrote by [Mohamad Saad Eddin](https://github.com/MHD22).
***you can visit my profile and follow me.❤️😎***
### ______________________________________________


###### Thanks for your time, I hope that you have enjoyed it.

###### [resource1](https://developer.android.com/training/testing/espresso)
###### [resource2](https://developer.android.com/training/testing/espresso/basics)
###### [resource3](https://developer.android.com/training/testing/espresso/recipes)
###### [resource4](https://developer.android.com/studio/test/espresso-test-recorder)