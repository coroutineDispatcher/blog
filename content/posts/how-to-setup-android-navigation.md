---
title: 'How to setup Android Navigation Component with Drawer'
date: 2019-06-19T10:47:00.002+02:00
draft: false
aliases: [ "/2019/06/how-to-setup-android-navigation.html" ]
author: "Stavro Xhardha"
---

How to setup Android Navigation Component with Drawer . \* { font-family: Georgia, Cambria, "Times New Roman", Times, serif; } html, body { margin: 0; padding: 0; } h1 { font-size: 50px; margin-bottom: 17px; color: #333; } h2 { font-size: 24px; line-height: 1.6; margin: 30px 0 0 0; margin-bottom: 18px; margin-top: 33px; color: #333; } h3 { font-size: 30px; margin: 10px 0 20px 0; color: #333; } header { width: 640px; margin: auto; } section { width: 640px; margin: auto; } section p { margin-bottom: 27px; font-size: 20px; line-height: 1.6; color: #333; } section img { max-width: 640px; } footer { padding: 0 20px; margin: 50px 0; text-align: center; font-size: 12px; } .aspectRatioPlaceholder { max-width: auto !important; max-height: auto !important; } .aspectRatioPlaceholder-fill { padding-bottom: 0 !important; } header, section\[data-field=subtitle\], section\[data-field=description\] { display: none; }

It’s been a wile since Android team launched the new Navigation style in Android . For me , it looks amazingly cool and I thought I should…

* * *

![](https://cdn-images-1.medium.com/max/800/1*j_omB_-m37BMK1KXmcK7Tw.jpeg)

It’s been a wile since Android team launched the new Navigation style in Android. For me , it looks amazingly cool and I thought I should share it with you.

Let’s start :

**First of all , let’s start with setting up the navigation dependencies :**

```
implementation "androidx.navigation:navigation-fragment-ktx:$rootProject.navigationVersion"  
implementation "androidx.navigation:navigation-ui-ktx:$rootProject.navigationVersion"
```

where `navigationVersion` is stored in the build.gradle project module:

```
ext {  
        navigationVersion = "2.0.0"  
        ...  
    }
```

**On with the show :   
**In your res menu , define a new directory named _navigation._

**After that, create a nav\_graph:**

```
<?xml version="1.0" encoding="utf-8"?>  
<navigation xmlns:android="http://schemas.android.com/apk/res/android"  
            xmlns:app="http://schemas.android.com/apk/res-auto"  
            xmlns:tools="http://schemas.android.com/tools" android:id="@+id/treasure_nav"  
            app:startDestination="@id/fragment1">
``````
 <fragment android:id="@+id/fragment1" android:name="com.stavro_xhardha.pockettreasure.Fragment1"  
              android:label="fragment_fragment1" tools:layout="@layout/fragment_fragment1">  
        <action android:id="@+id/action_fragment1_to_fragment2" app:destination="@id/fragment2"/>  
    </fragment>  
    <fragment android:id="@+id/fragment2" android:name="com.stavro_xhardha.pockettreasure.Fragment2"  
              android:label="fragment_fragment2" tools:layout="@layout/fragment_fragment2"/>  
</navigation>
```

which basically is the equivalent of this:

![](https://cdn-images-1.medium.com/max/800/0*7EFeTvVTe_Rh_zzd)

Just two fragments connected to each other. What a _destination_ is, is basically the view where I (or the user) needs to be. Since I will be needing to launch something when the app starts, I will define my `fragment1` as start destination (never use names like that).

**Setting up the _main\_activity_ view:**

```
<?xml version="1.0" encoding="utf-8"?>  
<androidx.drawerlayout.widget.DrawerLayout  
        xmlns:android="http://schemas.android.com/apk/res/android"  
        xmlns:app="http://schemas.android.com/apk/res-auto"  
        xmlns:tools="http://schemas.android.com/tools"  
        android:id="@+id/drawer_layout"  
        android:layout_width="match_parent"  
        android:layout_height="match_parent"  
        android:fitsSystemWindows="true"  
        tools:context=".MainActivity"  
        tools:openDrawer="start">
``````
 <LinearLayout  
            android:layout_width="match_parent"  
            android:layout_height="match_parent"  
            android:orientation="vertical">
``````
 <androidx.appcompat.widget.Toolbar  
                android:id="@+id/toolbar"  
                android:layout_width="match_parent"  
                android:layout_height="wrap_content"  
                android:background="@color/colorPrimary"  
                android:theme="@style/ThemeOverlay.MaterialComponents.Dark.ActionBar"/>
``````
 <fragment  
                android:name="androidx.navigation.fragment.NavHostFragment"  
                android:id="@+id/nav_host_fragment"  
                android:layout_width="match_parent"  
                android:layout_height="match_parent"  
                app:defaultNavHost="true"  
                app:navGraph="@navigation/treasure_nav"/>  
    </LinearLayout>
``````
 <com.google.android.material.navigation.NavigationView  
            android:id="@+id/nav_view"  
            app:menu="@menu/activity_main_drawer"  
            android:layout_width="wrap_content"  
            android:layout_height="match_parent"  
            android:layout_gravity="start"  
            android:fitsSystemWindows="true"/>
``````
</androidx.drawerlayout.widget.DrawerLayout>
```

Of course , we need these things to setup :

*   The `drawer_layout`, responsible for holding my menu.
*   The `toolbar`, responsible for holding my top left icon of the drawer.
*   The `nav_host_fragment` which will hold any view that I will load from the Navigation logic.
*   The `nav_view` responsible for my navigation.

**Am I done ? Time to setup the logics:**

```
class MainActivity : AppCompatActivity(),  
    AppBarConfiguration.OnNavigateUpListener {
``````
 private lateinit var appBarConfiguration: AppBarConfiguration;
``````
 override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        setContentView(R.layout.activity_main)
``````
 val toolbar = findViewById<Toolbar>(R.id.toolbar)  
        setSupportActionBar(toolbar) //set the toolbar
``````
 val host: NavHostFragment = supportFragmentManager  
            .findFragmentById(R.id.nav_host_fragment) as NavHostFragment? ?: return  
        val navController = host.navController 
``````
 appBarConfiguration = AppBarConfiguration(navController.graph) //configure nav controller
``````
 setupNavigation(navController) //setup navigation 
``````
 setupActionBar(navController, appBarConfiguration) // setup action bar
``````
 //hear for event changes  
        navController.addOnDestinationChangedListener { _, destination, _ ->  
            val dest: String = try {  
                resources.getResourceName(destination.id)  
            } catch (e: Resources.NotFoundException) {  
                Integer.toString(destination.id)  
            }
``````
 Toast.makeText(  
                this@MainActivity, "Navigated to $dest",  
                Toast.LENGTH_SHORT  
            ).show()  
            Log.d("NavigationActivity", "Navigated to $dest")  
        }  
    }
``````
 private fun setupActionBar(  
        navController: NavController,  
        appBarConfig: AppBarConfiguration  
    ) {  
        setupActionBarWithNavController(navController, appBarConfig)  
    }
``````
 private fun setupNavigation(navController: NavController) {  
        val sideNavView = findViewById<NavigationView>(R.id.nav_view)  
        sideNavView?.setupWithNavController(navController)
``````
 val drawerLayout: DrawerLayout? = findViewById(R.id.drawer_layout)  
          
        //fragments load from here but how ?   
        appBarConfiguration = AppBarConfiguration(  
            setOf(R.id.fragment1, R.id.fragment2),  
            drawerLayout  
        )  
    }
``````
 override fun onCreateOptionsMenu(menu: Menu): Boolean {  
        val retValue = super.onCreateOptionsMenu(menu)  
        val navigationView = findViewById<NavigationView>(R.id.nav_view)  
        if (navigationView == null) {  
            //android needs to know what menu I need  
            menuInflater.inflate(R.menu.activity_main_drawer, menu)  
            return true  
        }  
        return retValue  
    }
``````
 override fun onOptionsItemSelected(item: MenuItem?): Boolean {  
        //I need to open the drawer onClick  
        when (item!!.itemId) {  
            android.R.id.home ->  
                drawer_layout.openDrawer(GravityCompat.START)  
        }  
        return item.onNavDestinationSelected(findNavController(R.id.nav_host_fragment))  
                || super.onOptionsItemSelected(item)  
    }
``````
 override fun onBackPressed() {  
        //the code is beautiful enough without comments   
        if (drawer_layout.isDrawerOpen(GravityCompat.START)) {  
            drawer_layout.closeDrawer(GravityCompat.START)  
        } else {  
            super.onBackPressed()  
        }  
    }
``````
}
```

Hope you noticed my comment here

> //fragments load from here but how?

Well this is what I call great.  
I don’t even need to define which menu goes where with the new navigation.  
**But how?**   
Go back to the MainActivity and you will notice this line of code: `resources.getResourceName(destination.id)`.  
Satisfied enough?

Just place the same id that you added in the _nav\_graph ,_ and it will work fine.

```
<?xml version="1.0" encoding="utf-8"?>  
<menu xmlns:android="http://schemas.android.com/apk/res/android"  
      xmlns:tools="http://schemas.android.com/tools"  
      tools:showIn="navigation_view">
``````
 <group android:checkableBehavior="single">  
        <item  
                android:id="@+id/fragment1" //here , check the id on the nav_graph now :)   
                android:icon="@drawable/ic_names_of_god"  
                android:title="@string/names_of_god"/>  
        <item  
                android:id="@+id/fragment2"  
                android:icon="@drawable/ic_quran"  
                android:title="@string/quran"/>  
        <item  
                android:id="@+id/nav_calendar"  
                android:icon="@drawable/ic_calendar"  
                android:title="@string/calendar_and_events"/>
``````
 <item  
                android:id="@+id/nav_tasbeeh"  
                android:icon="@drawable/ic_tasbeeh"  
                android:title="@string/tasbeeh"/>
``````
 <item  
                android:id="@+id/nav_gallery"  
                android:icon="@drawable/ic_menu_gallery"  
                android:title="@string/gallery"/>
``````
 <item  
                android:id="@+id/nav_news"  
                android:icon="@drawable/ic_news"  
                android:title="@string/news"/>
``````
 <item  
                android:id="@+id/nav_settings"  
                android:icon="@drawable/ic_settings_dark_grey_24dp"  
                android:title="@string/action_settings"/>  
    </group>
``````
 <item android:title="@string/more">  
        <menu>  
            <item  
                    android:id="@+id/nav_finish"  
           android:icon="@drawable/ic_exit_to_app_dark_grey_24dp"  
                    android:title="@string/finish"/>  
        </menu>  
    </item>
``````
</menu>
```

Another thing to note here is the `appBarConfiguration = AppBarConfiguration( setOf(R.id.fragment1, R.id.fragment2),  
 drawerLayout )` line. Setting both of my `fragment1` and `fragment2` like that, it means that they will remain on the backstack after navigating to one another. That’s called placing as _top level destinations_.

Happy Navigating.

By [Stavro Xhardha](https://medium.com/@coroutinedispatcher) on [April 23, 2019](https://medium.com/p/88b833312661).

[Canonical link](https://medium.com/@coroutinedispatcher/how-to-setup-android-navigation-component-with-drawer-88b833312661)

Exported from [Medium](https://medium.com) on June 19, 2019.