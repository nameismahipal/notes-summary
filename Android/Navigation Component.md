# Navigation Component

https://codelabs.developers.google.com/codelabs/android-navigation/#1

The Navigation Component introduces the concept of a destination. A destination is any place you can navigate to in your app, usually a fragment or an activity.

The Navigation component also ensures a consistent and predictable user experience by adhering to an established set of principles.

1. First Principle. 

   - There is always a Starting place.

2. Second Principle
   
   - You can always go back

    - Navigation stack is a LIFO - last in first out structure.

    - Start destination is at the bottom of the stack.
    - Current destination is at the top of the stack.

      Operations, that change navigation stack, should always operate on the top of the stack, either by pushing/popping.

2. Third Principle  
  
    - Up button goes back (mostly)

    -  If on Start destination, there should be no up button. 
  Up button acts same as Back button, except that Back button can also take you to launcher or to another app.

The Navigation component consists of three key parts that are described below:

1. Navigation Graph (```res/navigation/navigation.xml```)
    > An XML Resource that contains all navigation destinations at single location. All individual content areas are called destinations.

    >To be declared in manifest.xml (If there is any deep-link/widgets)
    ```
    <navigation xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:app="http://schemas.android.com/apk/res-auto"
            xmlns:tools="http://schemas.android.com/tools"
    app:startDestination="@+id/home_dest">

    <!-- ...tags for fragments and activities here -->

    </navigation>
    ```
    ```
    1. <navigation> is the root node of every navigation graph.
    2. <navigation> contains one or more destinations, represented by <activity> or <fragment> elements.
    3. app:startDestination is an attribute that specifies the destination that is launched by default when the user first opens the app.
    ```

2. NavHost (```create <fragment> view in activity ```)
    >An empty container that displays destinations from navigation graph. Navigation component contains a default NavHost implementation, [NavHostFragment](https://developer.android.com/reference/androidx/navigation/fragment/NavHostFragment.html) that displays destinations. </br> When using fragments as destinations, the NavHostFragment automatically adds the FragmentNavigator class to its NavController.

    ```
    <fragment
            android:id="@+id/my_nav_host_fragment"
            android:name="androidx.navigation.fragment.NavHostFragment"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:defaultNavHost="true"  //this will intercept system back key
            app:navGraph="@navigation/navigation"
    ```
    this (nav host) fragment replaces itself with different destinations.

    ```
    1. android:id defines an ID for the fragment that you can use to reference the destination elsewhere in this XML and your code.
    2. android:name declares the fully qualified class name of the fragment to instantiate when you navigate to that destination.
    3. tools:layout specifies what layout should be shown in the graphical editor.
    ```

3. NavController
    >Manages app navigation(swapping of destinations) within a NavHost. Keeps track of the current position within Navigation graph. </br></br>NavControllers support leaving the navigation graph by navigating to another activity using the ActivityNavigator class and its nested ActivityNavigator.Destination class
 
    Depending on whether you're calling the navigation command from within a **fragment, activity or view**:
    (few ways to get a NavController object associated with your NavHostFragment)
    ``` 
    (Kotlin)
    - Fragment.findNavController()
    - View.findNavController()
    - Activity.findNavController(viewId: Int)
    ```

Navigatin Component Benifits: 

  - Handling fragment transactions 
  - Up/Back actions by default
  - animations/transitions
  - deep linking
  - Navigation UI patterns (navigation drawers, bottom nav)
  - [Safe Args](https://developer.android.com/guide/navigation/navigation-pass-data#Safe-args)- gradle plugin for type safety for passing data b/n destinations. </br>
  SafeArgs generates simple object ( that has generated setters and getters) and builder classes for type-safe access to arguments specified for destinations and actions. 

### **Gradle dependencies:**  

**Navigation**

App Build Gradle: 
```
implementation "androidx.navigation:navigation-fragment-ktx:$rootProject.navigationVersion"

implementation "androidx.navigation:navigation-ui-ktx:$rootProject.navigationVersion"
```

NavigationUI has static methods that associate menu items with navigation destinations, and navigation-ui-ktx is a set of extension functions that do the same. If NavigationUI finds a menu item with the same ID as a destination on the current graph, it configures the menu item to navigate to that destination.

**Safe Args**

Prj Build Gradle: 
```
classpath "androidx.navigation:navigation-safe-args-gradle-plugin:$navigationVersion"
```
App Build Gradle
```
apply plugin: 'kotlin-kapt'
apply plugin: 'androidx.navigation.safeargs'
```
## **How to Navigate ?** 
Navigation to different destinations can be done by 3 ways(by using the parameters created in Nav Graph):

1. via Action Id's
2. via Destination Id's
3. via SafeArgs Direction classes

In Nav Graph, 
    
- each destination (i.e., fragment) shall have an **id** called Destination Id. 
    - Ex: android:id="@+id/secondFragment"
    - This will invoke Fragment class mentioned at android:name=, and layout inflation within that class.
- each destination (i.e., fragment) may have an **< action> </ action>** set with an **id** called Action Id. Action tag will also set the destination.
    - Ex: android:id="@+id/action_firstFragment_to_secondFragment"
    
    - when a link is established from first fragment to second fragment, Nav Editor creates **action** tag within fragment of id/firstFragment
     
    **Example 1:**
    ```
    <action
        android:id="@+id/action_firstFragment_to_secondFragment"
        app:destination="@id/secondFragment" />
    ```

**Example 2:**
```
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/nav_root"
    app:startDestination="@+id/firstFragment">
    <fragment
        android:id="@+id/firstFragment"
        android:name="com.example.android.navigation.FirstFragment"
        android:label="@string/sampleLableText"
        tools:layout="@layout/fragment_first">
        <action
            android:id="@+id/action_firstFragment_to_secondFragment"
            app:destination="@id/secondFragment" />
    </fragment>
    <fragment
        android:id="@+id/secondFragment"
        android:name="com.example.android.navigation.SecondFragment"
        android:label="@string/sampleLableText"
        tools:layout="@layout/fragment_second">
        <action
            android:id="@+id/action_secondFragment_to_fourthFragment"
            app:destination="@id/fourthFragment" />
        <action
            android:id="@+id/action_secondFragment_to_thirdFragment"
            app:destination="@id/thirdFragment" />
    </fragment>
    <fragment
        android:id="@+id/thirdFragment"
        android:name="com.example.android.navigation.GameWonFragment"
        android:label="@string/sampleLableText"
        tools:layout="@layout/fragment_game_third">

        <argument
            android:name="numIndex"
            app:argType="integer"
            android:defaultValue="1"/>
        <argument
            android:name="numValue"
            app:argType="integer"
            android:defaultValue="99"/>
    </fragment>
    <fragment
        android:id="@+id/fourthFragment"
        android:name="com.example.android.navigation.GameOverFragment"
        android:label="@string/sampleLableText"
        tools:layout="@layout/fragment_game_fourth">
    </fragment>
</navigation>
```

when a link is established, action tag is created.

Nav Controller is needed to navigate b/n different destinations.

Nav host fragment is the parent of the view hierarchy(destinations). So any View, (including button view in a destination) can be used to find the Nav host fragment.

Navigation provides Helper class called **findNavController()** to find the enclosing Nav Host Fragment and returns the Navigation controller for that NavHost.

## **Navigation via Action Id**

```
binding.sampleButton.setOnClickListener {
    view: View -> Navigation.findNavController(view).navigate(R.id.action_firstFragment_to_secondFragment)
}
```
This can be further simplified, using kotlin extensions on View class
```
binding.sampleButton.setOnClickListener {
    view: View -> view.findNavController().navigate(R.id.action_firstFragment_to_secondFragment)
}
```
This can be further simplified.
Navigation can create onClickListener.

```
binding.sampleButton.setOnClickListener(Navigation.createNavigateOnClickListener(R.id.action_firstFragment_to_secondFragment, null))
```
OR
```
view.findViewById<Button>(R.id.navigate_action_button).setOnClickListener(Navigation.createNavigateOnClickListener(R.id.action_firstFragment_to_secondFragment, null))
```

#### Conditional Navigation

Set more than one action ids.
Like in Example 2 (above), =id/secondFragment has 2 action id each with different destination.

#### Back Stack (Pop Behaviour)

For each destination, set the Pop To behaviour.

If inclusive is set, it removes every thing in backstack including the reference fragment also which is set.

If inclusive is NOT set, it removes every thing in backstack untill the reference fragment.

#### UP Button

Navigation Controller has UI library, called NavigationUI. 
Navigation UI also integrates with Action Bar.

NavigationUI needs access to Nav Controller to attach Navigation to the Action Bar.

Preferred way to find the Nav controller is to use findNavController() and pass this activity and Nav Host fragment.

Using Ktx (Kotlin extension), it can be simplied to this.findNavController(R.id.myNavHostFragment)

```
val binding = DataBindingUtil.setContentView<ActivityMainBinding>(this, R.layout.activity_main)
val navController = this.findNavController(R.id.myNavHostFragment)
NavigationUI.setupActionBarWithNavController(this, navController)
```
```
override fun onSupportNavigateUp(): Boolean {
   val navController = this.findNavController(R.id.myNavHostFragment)
   return navController.navigateUp()
}
```

#### Menu 
(res/menu/overflow_menu.xml)

- Each Menu Item should have an Id. 
- Id should exactly be same as the fragment id mentioned in the Nav Graph.
- // Menu -- tell android that there is a menu
   setHasOptionsMenu(true)
- inflate options menu in onCreateOptionsMenu()
- handle navigation in onOptionsItemSelected()

https://gist.github.com/sayaMahi/6de6cdc9c5202c099343e7bb6e2d0237

## **Navigation via Destination Id**
```
val button = view.findViewById<Button>(R.id.navigate_destination_button)
        button?.setOnClickListener {
            findNavController().navigate(R.id.action_firstFragment_to_secondFragment)
}
```

## **Navigation via SafeArgs Direction classes**


### **SafeArgs**

https://developer.android.com/guide/navigation/navigation-pass-data#Safe-args

Fragments contain Arguments in the form of Android Bundle.

<img src=images/bundle-arguments.png width=50% heigh=25%>

This can cause issues if there is a null value or a different type, while unbundling at the destination fragment.

```
val username = arguments?.getString("usernameKey")
```

Safe args allows you to get rid of code like this when passing values between destinations.

So, traditional way can be replaced with, code that has generated setters and getters.

Ex:    
```
val username = args.username
```
<strong>Using Safe Args plugin, 

it generates class called Direction Class, which will have setters/getters, to represent Navigation from all our actions.</strong>

If ThirdFragment takes 2 arguments, as in Example 3, then arguments can be passed from SecondFragment to ThirdFragment as below

```
view.findNavController().navigate(SecondFragmentDirections.actionSecondFragmentToThirdFragment(int1stArg,int2ndArg))

view.findNavController().navigate(SecondFragmentDirections.actionSecondFragmentToFourthFragment())
```

**Example 3** 
```
<fragment
id/ThirdFragment>
...
<argument
    android:name="numIndex"
    app:argType="integer"
    android:defaultValue="1"/>
<argument
    android:name="numValue"
    app:argType="integer"
    android:defaultValue="99"/>    
</fragment>
```

Using the ```<argument>``` tag, safeargs generates a class called **ThirdFragmentArgs**. 

Args can be accessed as,

```
var safeArgs = ThirdFragmentArgs.fromBundle(arguments)

val index = safeArgs.numIndex 
val value = safeArgs.numValue 
```
Or in a Safe way as
```
val safeArgs: ThirdFragmentArgs by navArgs()
val index = safeArgs.numIndex
val value = safeArgs.numValue 
```
    
Since the XML includes an argument called **numIndex**, specified by **android:name="numIndex"**, the generated class **ThirdFragmentArgs** will include a variable **numIndex** with getters and setters.

### **Safe Args Direction classes**

You can also use safe args to navigate in a type safe way, with or without adding arguments. You do this using the generated Directions classes.


Navigation vis SafeArgs
 ```
view.findViewById<Button>(R.id.navigate_action_button)?.setOnClickListener{
    val action = SecondFragmentDirections.action_secondFragment_to_thirdFragment()
    action.setNumIndex(1)
    findNavController().navigate(action)
}
```

### **Navigating using menus, drawers and bottom navigation**

The Navigation Components include a NavigationUI class and the navigation-ui-ktx kotlin extensions. 

- NavigationUI has static methods that associate menu items with navigation destinations, and
- navigation-ui-ktx is a set of extension functions that do the same. 


   
If NavigationUI finds a menu item with the same ID as a destination on the current graph, it configures the menu item to navigate to that destination.

AppBarConfiguration: The purpose of AppBarConfiguration is to specify the configuration options you want for your toolbars, collapsing toolbars, and action bars. Configuration options include whether the bar must handle a drawer layout and which destinations are considered top-level destinations(so that Nav icon is shwon only when top-level destinations are active).

```
override fun onOptionsItemSelected(item: MenuItem): Boolean {

return item.onNavDestinationSelected(findNavController(R.id.my_nav_host_fragment))|| super.onOptionsItemSelected(item)

}
```

```
val host: NavHostFragment = supportFragmentManager
            .findFragmentById(R.id.my_nav_host_fragment) as NavHostFragment? ?: return

//Set up Action Bar
val navController = host.navController

setupBottomNavMenu(navController)    

private fun setupBottomNavMenu(navController: NavController) {
      val bottomNav = findViewById<BottomNavigationView>(R.id.bottom_nav_view)

    bottomNav?.setupWithNavController(navController)
}
```

#### Deep Links and Navigation

Navigation components also include deep link support. 

Deep links are a way to jump into the middle of your app's navigation, whether that's from an actual URL link or a pending intent from a notification.

One benefit of using the navigation library to handle deep links is that it ensures users start on the right destination with the appropriate back stack from other entry points such as app widgets, notifications, or web links.

Navigation provides a NavDeepLinkBuilder class to construct a PendingIntent that will take the user to a specific destination.

```
  val args = Bundle()
  args.putString("myarg", "From Widget");

  val pendingIntent = NavDeepLinkBuilder(context)
        .setGraph(R.navigation.mobile_navigation)
        .setDestination(R.id.deeplink_dest)
        .setArguments(args)
        .createPendingIntent()

  remoteViews.setOnClickPendingIntent(R.id.deep_link_button, pendingIntent)
```

- setGraph includes the navigation graph.
- setDestination specifies where the link goes to.
- setArguments includes any arguments you want to pass into your deep link.

 ##### Associating a web link with a destination

One of the most common uses of a deep link is to allow a web link to open an activity in your app. Traditionally you would use an intent-filter and associate a URL with the activity you want to open.

```<deepLink>``` is an element you can add to a destination in your graph. Each ```<deepLink>``` element has a single required attribute: ```app:uri```.

In addition to a direct URI match, the following features are supported:

URIs without a scheme are assumed to be http and https. For example, ```www.example.com``` will match ```http://www.example.com``` and ```https://www.example.com```.

You can use placeholders in the form of {placeholder_name} to match one or more characters. The String value of the placeholder is available in the arguments Bundle which has a key of the same name. For example, ```http://www.example.com/users/{id}``` will match ```http://www.example.com/users/4```.
You can use the .* wildcard to match zero or more characters.
NavController will automatically handle ACTION_VIEW intents and look for matching deep links.

mobile_navigation.xml (nav-graph)
```
<fragment
    ....>

    <argument
        ...>

    <deepLink app:uri="www.example.com/{myarg}" />
</fragment>
```
manifest.xml
```
//Add the Nav-graph tag

<activity android:name=".MainActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>

    <nav-graph android:value="@navigation/mobile_navigation" />
</activity>

```