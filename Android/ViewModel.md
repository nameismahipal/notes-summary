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

 AndroidViewModel class is the same as ViewModel, but it takes the application context as a parameter and makes it available as a property.

 To create Constructor for ViewModel, ViewModel Factory has to be used. This allows to seperate (creating) instances away from ViewModel. This way, It will be easy to test ViewModel.

 If using database, creating database instance will be in ViewModelFactory, which will be passed to ViewModel.

 

