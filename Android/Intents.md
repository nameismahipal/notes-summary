
# Intents

2 types

- Explicit 
  > Ex: launches Activities based on its class name
- Implicit
  > Ex: launches Activities based on parameters, such as action, category/type.

In Manifest file, 

Activities launched Explicity will be declared with just activity tag.

Activities launched Implicity will have Intent-filter declared.

Ex:
```
<activity
  android:name=".MainActivity"
  android:configChanges="orientation|screenSize|smallestScreenSize|screenLayout">
  
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>

 </activity>
```

```<intent-filter>``` is used to expose that activity can respond to implicit intents with certain action, category/type.

# Share Intent

```
class ThirdFragment : Fragment() {

override fun onCreateView(inflater: LayoutInflater, 
                          container: ViewGroup?,
                          savedInstanceState: Bundle?): View? {

    ...
 
    setHasOptionsMenu(true)
    
    return binding.root
}

private fun getShareIntent() : Intent {

    val args = GameWonFragmentArgs.fromBundle(arguments)
    
    val shareIntent = Intent(Intent.Action)
    
    shareIntent.setType("text/plain")
               .putExtra(Intent.EXTRA_TEXT, getString(R.string.share_text, args.numIndex, args.numValue))

    return shareIntent
}

private fun shareSuccess() {
  startActivity(getShareIntent())
}

override fun onOptionsItemSelected(item: MenuItem?): Boolean {
    when (item!!.itemId) {
            R.id.share -> shareSuccess()
    }
    return super.onOptionsItemSelected(item)
}
```
```<string name="share_text">This text is formated with %1$d/%2$d arguments!</string>```

This can be further simplified, using **Share Compat**

### Share Compat
This has method chaining, which has better readability.
```
private fun getShareIntent() : Intent {
        val args = GameWonFragmentArgs.fromBundle(arguments)

        return ShareCompat.IntentBuilder.from(activity)
                .setText(getString(R.string.share_success_text, args.numCorrect, args.numQuestions))
                .setType("text/plain")
                .intent
    }
```    

If there is only one activity that can handle the intent, it will be automatically selected without any chooser.

If there are many that can handle, then Chooser will be shown for the User to select.

Incase if No activity can handle the intent, then the code will crash without proper conditions.

getShareIntent() can be checked by the PackageManager System Service to see where the intent resolves to an Activity by resolveActivity. If it returns null, we can decide to make it invisible or stop inflating.

```
 override fun onCreateOptionsMenu(menu: Menu?, inflater: MenuInflater?) {
        super.onCreateOptionsMenu(menu, inflater)
        inflater?.inflate(R.menu.winner_menu, menu)

        // check if the activity resolves
        if (null == getShareIntent().resolveActivity(activity!!.packageManager)) {
            // hide the menu item if it doesn't resolve
            menu?.findItem(R.id.share)?.setVisible(false)
        }
    }
```