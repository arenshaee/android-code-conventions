# Code Conventions

 * Maximum size of a function should be 25 lines.
 * Avoid Hardcoding.
 * Use **Tabs** for indentions and spaces only for formation.
 * No more than **40 changed files** in pull requests. If your PR exceeds this number, split into smaller PRs.

For All **"android related"** best practices, stick with [this documentation](https://github.com/futurice/android-best-practices).

(*Section below is copied from [here](https://github.com/ribot/android-guidelines/blob/master/project_and_code_guidelines.md) with some changes.*)
# Project guidelines

## Project structure

New projects should follow the Android Gradle project structure that is defined on the [Android Gradle plugin user guide](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Project-Structure). The [ribot Boilerplate](https://github.com/ribot/android-boilerplate) project is a good reference to start from.

## File naming

### Class files
Class names are written in [UpperCamelCase](http://en.wikipedia.org/wiki/CamelCase).

For classes that extend an Android component, the name of the class should end with the name of the component; for example: `SignInActivity`, `SignInFragment`, `ImageUploaderService`, `ChangePasswordDialog`.

### Resources files

Resources file names are written in __lowercase_underscore__.

#### Drawable files

Naming conventions for drawables:


| Asset Type   | Prefix            |		Example               |
|--------------| ------------------|-----------------------------|
| Action bar   | `ab_`             | `ab_stacked.9.png`          |
| Button       | `btn_`	            | `btn_send_pressed.9.png`    |
| Dialog       | `dialog_`         | `dialog_top.9.png`          |
| Divider      | `divider_`        | `divider_horizontal.9.png`  |
| Icon         | `ic_`	            | `ic_star.png`               |
| Menu         | `menu_	`           | `menu_submenu_bg.9.png`     |
| Notification | `notification_`	| `notification_bg.9.png`     |
| Tabs         | `tab_`            | `tab_pressed.9.png`         |

<br/>

| Asset Type                      | Prefix             | Example                      |
| --------------------------------| ----------------   | ---------------------------- |
| Icons                           | `ic_`              | `ic_star.png`                |
| Launcher icons                  | `ic_launcher`      | `ic_launcher_calendar.png`   |
| Menu icons and Action Bar icons | `ic_menu`          | `ic_menu_archive.png`        |
| Status bar icons                | `ic_stat_notify`   | `ic_stat_notify_msg.png`     |
| Tab icons                       | `ic_tab`           | `ic_tab_recent.png`          |
| Dialog icons                    | `ic_dialog`        | `ic_dialog_info.png`         |

Naming conventions for selector states:

| State	       | Suffix          | Example                     |
|--------------|-----------------|-----------------------------|
| Normal       | `_normal`       | `btn_order_normal.9.png`    |
| Pressed      | `_pressed`      | `btn_order_pressed.9.png`   |
| Focused      | `_focused`      | `btn_order_focused.9.png`   |
| Disabled     | `_disabled`     | `btn_order_disabled.9.png`  |
| Selected     | `_selected`     | `btn_order_selected.9.png`  |


####  Layout files

Layout files should match the name of the Android components that they are intended for but moving the top level component name to the beginning. For example, if we are creating a layout for the `SignInActivity`, the name of the layout file should be `activity_sign_in.xml`.

| Component        | Class Name             | Layout Name                   |
| ---------------- | ---------------------- | ----------------------------- |
| Activity         | `UserProfileActivity`  | `activity_user_profile.xml`   |
| Fragment         | `SignUpFragment`       | `fragment_sign_up.xml`        |
| Dialog           | `ChangePasswordDialog` | `dialog_change_password.xml`  |
| AdapterView item | ---                    | `item_person.xml`             |
| Partial layout   | ---                    | `partial_stats_bar.xml`       |

A slightly different case is when we are creating a layout that is going to be inflated by an `Adapter`, e.g to populate a `ListView`. In this case, the name of the layout should start with `item_`.

Note that there are cases where these rules will not be possible to apply. For example, when creating layout files that are intended to be part of other layouts. In this case you should use the prefix `partial_`.

#### Menu files

Similar to layout files, menu files should match the name of the component. For example, if we are defining a menu file that is going to be used in the `UserActivity`, then the name of the file should be `menu_activity_user.xml`

#### Values files

Resource files in the values folder should be __plural__, e.g. `strings.xml`, `styles.xml`, `colors.xml`, `dimens.xml`, `attrs.xml`

# Code guidelines

For **ONLY** kotlin Naming Conventions, follow [the guidelines](https://kotlinlang.org/docs/coding-conventions.html#control-flow-statements). for other things just do what makes you fell comfortable or decide with your teammates about it.
-   fields start with a lower case letter.
-   All constants are ALL_CAPS_WITH_UNDERSCORES.

### Treat acronyms as words

| Good           | Bad            |
| -------------- | -------------- |
| `XmlHttpRequest` | `XMLHTTPRequest` |
| `getCustomerId`  | `getCustomerID`  |
| `String url`     | `String URL`     |
| `long id`        | `long ID`        |

### Limit variable scope

_The scope of local variables should be kept to a minimum (Effective Java Item 29). By doing so, you increase the readability and maintainability of your code and reduce the likelihood of error. Each variable should be declared in the innermost block that encloses all uses of the variable._

_Local variables should be declared at the point they are first used. Nearly every local variable declaration should contain an initializer. If you don't yet have enough information to initialize a variable sensibly, you should postpone the declaration until you do._  - ([Android code style guidelines](https://source.android.com/source/code-style.html#limit-variable-scope))

### Logging guidelines

Use the logging methods provided by the `Log` class to print out error messages or other information that may be useful for developers to identify issues:

* `Log.v(tag: String, msg: String)` (verbose)
* `Log.d(tag: String, msg: String)` (debug)
* `Log.i(tag: String, msg: String)` (information)
* `Log.w(tag: String, msg: String)` (warning)
* `Log.e(tag: String, msg: String)` (error)

As a general rule, we use the class name as tag and we define it as a `val` field `companion object` section of a file. keep in mind that TAGs need to have their type defined. For example:

```kotlin
class MyClass {
    fun myMethod() {
		Log.e(TAG, "My error message")
    }
    
    companion object {
		val TAG: String = MyClass::class.java.simpleName
    }
}
```

VERBOSE and DEBUG logs __must__ be disabled on release builds. It is also recommended to disable INFORMATION, WARNING and ERROR logs but you may want to keep them enabled if you think they may be useful to identify issues on release builds. If you decide to leave them enabled, you have to make sure that they are not leaking private information such as email addresses, user ids, etc.

To only show logs on debug builds:

```kotlin
if (BuildConfig.DEBUG) Log.d(TAG, "The value of x is " + x)
```

### Parameter ordering in methods

When programming for Android, it is quite common to define methods that take a  `Context`. If you are writing a method like this, then the  **Context**  must be the  **first**  parameter. The opposite case are **anonymous functions** or **callback** interfaces that should always be the **last** parameter.

### String constants, naming, and values

Many elements of the Android SDK such as `SharedPreferences`, `Bundle`, or `Intent` use a key-value pair approach so it's very likely that even for a small app you end up having to write a lot of String constants.

When using one of these components, you __must__ define the keys as a `static final` fields and they should be prefixed as indicated below.

| Element            | Field Name Prefix |
| -----------------  | ----------------- |
| SharedPreferences  | `PREF_`             |
| Bundle             | `BUNDLE_`           |
| Fragment Arguments | `ARGUMENT_`         |
| Intent Extra       | `EXTRA_`            |
| Intent Action      | `ACTION_`           |

Note that the arguments of a Fragment - `Fragment.arguments` - are also a Bundle. However, because this is a quite common use of Bundles, we define a different prefix for them.

Example:

```kotlin
// Note the value of the field is the same as the name to avoid duplication issues
companion object {
	const val PREF_EMAIL = "PREF_EMAIL";
	const val BUNDLE_AGE = "BUNDLE_AGE";
	const val ARGUMENT_USER_ID = "ARGUMENT_USER_ID";

// Intent-related items use full package name as value
	const val EXTRA_SURNAME = "com.myapp.extras.EXTRA_SURNAME";
	const val ACTION_OPEN_USER = "com.myapp.action.ACTION_OPEN_USER";
}
```

### Arguments in Fragments and Activities

When data is passed into an `Activity` or `Fragment` via an `Intent` or a `Bundle`, the keys for the different values __must__ follow the rules described in the section above.

When an `Activity` or `Fragment` expects arguments, it should provide a  static method that facilitates the creation of the relevant `Intent` or `Fragment`.

In the case of Activities the method is usually called `getStartIntent()`:

```kotlin
companion object {
	fun getStartIntent(context: Context, user: User): Intent {
		val intent = Intent(context, ThisActivity::class)
		intent.putParcelableExtra(EXTRA_USER, user)
		return intent
	}
}
```

For Fragments it is named `newInstance()` and handles the creation of the Fragment with the right arguments:

```kotlin
companion object {
	fun newInstance(user: User): UserFragment {
		val fragment = UserFragment()
		val args = Bundle()
		args.putParcelable(ARGUMENT_USER, user)
		fragment.arguments = args
		return fragment
	}
}
```

__Note__: If we provide the methods described above, the keys for extras and arguments should be `private` because there is not need for them to be exposed outside the class.

  
## XML style rules

### Use self closing tags

When an XML element doesn't have any contents, you __must__ use self closing tags.

This is good:

```xml
<TextView
	android:id="@+id/text_view_profile"
	android:layout_width="wrap_content"
	android:layout_height="wrap_content" />
```

This is __bad__ :

```xml
<!-- Don\'t do this! -->
<TextView
    android:id="@+id/text_view_profile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" >
</TextView>
```

### Resources naming

Resource IDs and names are written in __lowercase_underscore__.

#### ID naming

IDs should be prefixed with the name of the element in lowercase underscore. For example:


| Element            | Prefix            |
| -----------------  | ----------------- |
| `TextView`           | `<place>_<intent>_label`    |
| `ImageView`          | `<place>_<intent>_image`    |
| `Button`             | `<place>_<intent>_action`   |
| `EditText`           | `<place>_<intent>_input`    |
| `CheckBox`           | `<place>_<intent>_check`    |
| `ViewGroup`          | `<place>_<intent>_container`|


Image view example:

```xml
<ImageView
    android:id="@+id/fragment_foo_user_image_profile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
```

Menu example:

```xml
<menu>
	<item
        android:id="@+id/foo_done_menu"
        android:title="Done" />
</menu>
```

#### Strings

String names start with a prefix that identifies the section they belong to. For example `registration_email_hint` or `registration_name_hint`. If a string __doesn't belong__ to any section, then you should follow the rules below:


| Prefix             | Description                           |
| -----------------  | --------------------------------------|
| `error_`             | An error message                      |
| `msg_`               | A regular information message         |
| `title_`             | A title, i.e. a dialog title          |
| `action_`            | An action such as "Save" or "Create"  |

#### Styles and Themes

Unlike the rest of resources, style names are written in __UpperCamelCase__.


### Database and Room

Right all of your sql queries and commands with small case except for table names, column names, paramenters; for those you can use whatever they have been written with.

```sql
select * from User where id > :id
```

for naming DAO's just use the name of the entity, with `Dao` postfix: `UserDao`
for naming DAO's functions, use small camel case, with only the function name. avoid using the entity's name in function names:
```kotlin
// bad
fun getUserById(id: long): User
fun getAllUserList(): List<User>

// good
fun getById(id: long): User
fun getAll(): List<User>
```


