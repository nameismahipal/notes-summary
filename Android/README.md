## Navigation Component

The Navigation Component introduces the concept of a destination. A destination is any place you can navigate to in your app, usually a fragment or an activity.

The Navigation component also ensures a consistent and predictable user experience by adhering to an established set of principles.

1. First Principle. 

   - There is always a Starting place.

2. Second Principle
   
   - You can always go back

    Navigation stack is a LIFO - last in first out structure.

  - Start destination is at the bottom of the stack.
  - Current destination is at the top of the stack.

    Operations, that change navigation stack, should always operate on the top of the stack, either by pushing/popping.

2. Third Principle  
  
  - Up button goes back (mostly)

    If on Start destination, there should be no up button. 
  Up button acts same as Back button, except that Back button can also take you to launcher or to another app.

The Navigation component consists of three key parts that are described below:

1. Navigation Graph
    > An XML Resource that contains all navigation destinations at single location. All individual content areas are called destinations.

2. NavHost
    >An empty container that displays destinations from navigation graph. Navigation component contains a default NavHost implementation, [NavHostFragment](https://developer.android.com/reference/androidx/navigation/fragment/NavHostFragment.html) that displays destinations. </br> When using fragments as destinations, the NavHostFragment automatically adds the FragmentNavigator class to its NavController.

3. NavController
    >Manages app navigation(swapping of destinations) within a NavHost. Keeps track of the current position within Navigation graph. </br></br>NavControllers support leaving the navigation graph by navigating to another activity using the ActivityNavigator class and its nested ActivityNavigator.Destination class


Navigatin Component Benifits: 

  - Handling fragment transactions 
  - Up/Back actions by default
  - animations/transitions
  - deep linking
  - Navigation UI patterns (navigation drawers, bottom nav)
  - [Safe Args](https://developer.android.com/guide/navigation/navigation-pass-data#Safe-args)- gradle plugin for type safety for passing data b/n destinations. </br>
  SafeArgs generates simple object ( that has generated setters and getters) and builder classes for type-safe access to arguments specified for destinations and actions. 
    ```
    Gradle:
    //Navigation
    implementation "androidx.navigation:navigation-fragment-ktx:$rootProject.navigationVersion"
    implementation "androidx.navigation:navigation-ui-ktx:$rootProject.navigationVersion"
    ```

    Navigation via Destination
    ```
    val button = view.findViewById<Button>(R.id.navigate_destination_button)
        button?.setOnClickListener {
            findNavController().navigate(R.id.flow_step_one_dest)
        }
    ```

    Navigation via Actions
      ```
    view.findViewById<Button>(R.id.navigate_action_button).setOnClickListener(
                Navigation.createNavigateOnClickListener(R.id.next_action, null)
        )
    ```

    Navigation via SafeArgs

     // Below (via Direction Classes)

    #### SafeArgs

    Safe args allows you to get rid of code like this when passing values between destinations:
    
    ```val username = arguments?.getString("usernameKey")```
    
    And, instead, replace it with code that has generated setters and getters.
    
    ```val username = args.username```
 
    ```
    <fragment>
    ...
    <argument
        android:name="flowStepNumber"
        app:argType="integer"
        android:defaultValue="1"/>

    </fragment>
    ```

    Using the ```<argument>``` tag, safeargs generates a class called FlowStepFragmentArgs. 
    
    Since the XML includes an argument called **flowStepNumber**, specified by **android:name="flowStepNumber"**, the generated class **FlowStepFragmentArgs** will include a variable **flowStepNumber** with getters and setters.

    ```
    val safeArgs = FlowStepFragmentArgs.fromBundle(arguments)
    val flowStepNumber = safeArgs.flowStepNumber 
    ```

    #### Safe Args Direction classes

    You can also use safe args to navigate in a type safe way, with or without adding arguments. You do this using the generated Directions classes.


    Navigation vis SafeArgs
    ```
    view.findViewById<Button>(R.id.navigate_action_button)?.setOnClickListener{
        val action = HomeFragmentDirections.nextAction()
        action.setFlowStepNumber(1)
        findNavController().navigate(action)
    }
    ```

    #### Navigating using menus, drawers and bottom navigation

    The Navigation Components include a NavigationUI class and the navigation-ui-ktx kotlin extensions. 

    - NavigationUI has static methods that associate menu items with navigation destinations, and
    - navigation-ui-ktx is a set of extension functions that do the same. 
    
    If NavigationUI finds a menu item with the same ID as a destination on the current graph, it configures the menu item to navigate to that destination.
    ```
    override fun onOptionsItemSelected(item: MenuItem): Boolean {
    return item.onNavDestinationSelected(findNavController(R.id.my_nav_host_fragment))
            || super.onOptionsItemSelected(item)
    }
    ```

    ```
    val host: NavHostFragment = supportFragmentManager
              .findFragmentById(R.id.my_nav_host_fragment) as NavHostFragment? ?: return

        // Set up Action Bar
        val navController = host.navController

    
    setupBottomNavMenu(navController)    

    private fun setupBottomNavMenu(navController: NavController) {
      val bottomNav = findViewById<BottomNavigationView>(R.id.bottom_nav_view)

      bottomNav?.setupWithNavController(navController)
    }
    ```
#### Deep Links and Navigation

Navigation components also include deep link support. Deep links are a way to jump into the middle of your app's navigation, whether that's from an actual URL link or a pending intent from a notification.

One benefit of using the navigation library to handle deep links is that it ensures users start on the right destination with the appropriate back stack from other entry points such as app widgets, notifications, or web links.

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
```
mobile_navigation.xml (nav-graph)

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