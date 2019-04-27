ViewModel gets destroyed when Activity/Fragment gets destroyed completedly.

Before ViewModel gets destroyed, there is a call back called, in the ViewModel class, ```onCleared()```

Initialize ViewModel,
viewModel = ViewModelProviders.of(this).get(GameViewModel::class.java)

Transformations.map take one live data -> does some operation -> outputs another live data.

ex:   
val currentTimeString = Transformations.map(currentTime, {time ->
        DateUtils.formatElapsedTime(time)
    })

For more complex Transformations, use Switch Map or MediatorLiveData

 To create Constructor for ViewModel, ViewModel Factory has to be used. This allows to seperate (creating) instances away from ViewModel. This way, It will be easy to test ViewModel.

 If using database, creating database instance will be in ViewModelFactory, which will be passed to ViewModel.

 AndroidViewModel class is the same as ViewModel, but it takes the application context as a parameter and makes it available as a property.
 
 Never pass context into ViewModel instances. Do not store Activity, Fragment, or View instances or their Context in the ViewModel.

For example, an Activity can be destroyed and created many times during the lifecycle of a ViewModel as the device is rotated. If you store a reference to the Activity in the ViewModel, you end up with references that point to the destroyed Activity. This is a memory leak.

If you need the application context, use AndroidViewModel.

 

