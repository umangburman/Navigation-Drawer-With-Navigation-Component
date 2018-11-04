# Navigation Drawer with Navigation Component

This is an example of **Navigation Drawer with Navigation Architecture Component** in Android. I

The Navigation Architecture Component Part of Android Jetpack.

The Navigation Architecture Component simplifies the implementation of navigation in an Android app.

This section begins by providing the guiding principles of navigation as implemented by the Navigation Architecture Component. The principles are followed by Implementing navigation with the Navigation Architecture Component providing the fundamental tasks to implement the Navigation Architecture Component in your app. Finally, you can use it in our legacy Navigation Drawer, depending on your needs for your app:

So Let's Get Started:

1. Principles of Navigation.
2. What is Androidx?
3. What is destination, actions and NavHostFragment.
5. Implementation Step-by-Step.
5. Conclusion.

## **Principles of Navigation**

**Answer:** The goal of any in-app navigation should be to provide a consistent and predictable experience to users. To meet this goal, the Navigation Architecture Component helps you build an app that adheres to each of the navigation principles below:

**a.** The app should have a fixed starting destination.

**b.** A stack is used to represent the "navigation state" of an app.

**c.** The Up button never exits your app.

**d.** Up and Back are equivalent within your app's task

**e.** Deep linking to a destination or navigating to the same destination should yield the same stack.


## **What is Androidx?**

**Answer:** AndroidX is the open-source project that the Android team uses to develop, test, package, version and release libraries within Jetpack.

AndroidX is a major improvement to the original Android Support Library. Like the Support Library, AndroidX ships separately from the Android OS and provides backwards-compatibility across Android releases. AndroidX fully replaces the Support Library by providing feature parity and new libraries. In addition AndroidX includes the following features:

**a.** All packages in AndroidX live in a consistent namespace starting with the string androidx. The Support Library packages have been mapped into corresponding androidx.* packages. For a full mapping of all the old classes and build artifacts to the new ones, see the Package Refactoring page.

**b.** Unlike the Support Library, AndroidX packages are separately maintained and updated. The androidx packages use strict Semantic Versioning starting with version 1.0.0. You can update AndroidX libraries in your project independently.

**c.** All new Support Library development will occur in the AndroidX library. This includes maintenance of the original Support Library artifacts and introduction of new Jetpack components.

Easy way to migrate your project to Androidx:

<img src="https://pli.io/2M9zMn.png" />

Changes from legacy:

<img src="https://pli.io/2M93eJ.png" />


## **What is destination, actions and NavHostFragment**

**Answer:** 
**1. Destination**: A destination is any place you can navigate to in your app. While destinations are usually Fragments representing specific screens, the Navigation Architecture Component supports other destination types:

 - Activities
 - Navigation graphs and subgraphs - when the destination is a graph or subgraph, you navigate to the starting destination of that graph or subgraph
 - Custom destination types
 
 **2. Actions**: In addition to destinations, a navigation graph has connections between destinations called actions.
 
 <img src="https://developer.android.com/images/topic/libraries/architecture/navigation-graph.png" />
 
 In the above image, the visual representation of a navigation graph for a sample app containing 6 destinations connected by 5 actions.

**3. NavHostFragment**: NavHostFragment provides an area within your layout for self-contained navigation to occur.

NavHostFragment is intended to be used as the content area within a layout resource defining your app's chrome around it, e.g.:

```xml
<androidx.drawerlayout.widget.DrawerLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent">
  
    <fragment
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:id="@+id/my_nav_host_fragment"
            android:name="androidx.navigation.fragment.NavHostFragment"
            app:navGraph="@xml/nav_sample"
            app:defaultNavHost="true" />
  
    <com.google.android.material.navigation.NavigationView
        android:id="@+id/navigationView"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        android:fitsSystemWindows="true"
        app:headerLayout="@layout/header_layout"
        app:menu="@menu/drawer_menu" />
  
</androidx.drawerlayout.widget.DrawerLayout>
```
Each NavHostFragment has a NavController that defines valid navigation within the navigation host. This includes the navigation graph as well as navigation state such as current location and back stack that will be saved and restored along with the NavHostFragment itself.

NavHostFragments register their navigation controller at the root of their view subtree such that any descendant can obtain the controller instance through the Navigation helper class's methods such as Navigation.findNavController(View). View event listener implementations such as View.OnClickListener within navigation destination fragments can use these helpers to navigate based on user interaction without creating a tight coupling to the navigation host.


## **Implementation Step-by-Step**

### **Step1:** Take a new project and Refactor your app to support androidx as shown below:

<img src="https://pli.io/2M9zMn.png" />

### **Step2:** Add Navigation Architecture Component suppporting dependency in your app level Gradle:

```Java
def nav_version = "1.0.0-alpha06"

// Navigation components
implementation "android.arch.navigation:navigation-fragment:$nav_version"
implementation "android.arch.navigation:navigation-ui:$nav_version"
```

### **Step3:** Add navigatiion to your project:

1. Click File > Settings (Android Studio > Preferences on Mac), select the Experimental category in the left pane, check Enable Navigation Editor, and then restart Android Studio.

2. In the Project window, right-click on the res directory and select New > Android Resource File. The New Resource File dialog appears.

3. Type a name in the File name field, such as "nav_graph".

4. Select Navigation from the Resource type drop-down list.

5. Click OK. The following occurs:

 - A navigation resource directory is created within the res directory. 
 - A nav_graph.xml file is created within the navigation directory.
 - The nav_graph.xml file opens in the Navigation Editor. This xml file contains your navigation graph.
 
 6. Click the Text tab to toggle to the XML text view. The XML for an empty navigation graph looks like this:
 
 ```xml
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android">
  ...
</navigation>
```

7. Click Design to return to the Navigation Editor.

### **Step4:** Design your Fragments and your navigation should look like this:

```xml
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/nav_graph"
    app:startDestination="@id/defaultFragment">

    <fragment
        android:id="@+id/firstFragment"
        android:name="com.example.umangburman.navdrawerwithnavcomponent.FirstFragment"
        android:label="First Fragment"
        tools:layout="@layout/first_fragment" />
    <fragment
        android:id="@+id/secondFragment"
        android:name="com.example.umangburman.navdrawerwithnavcomponent.SecondFragment"
        android:label="Second Fragment"
        tools:layout="@layout/second_fragment" />
    <fragment
        android:id="@+id/thirdFragment"
        android:name="com.example.umangburman.navdrawerwithnavcomponent.ThirdFragment"
        android:label="Third Fragment"
        tools:layout="@layout/third_fragment" />
    <fragment
        android:id="@+id/defaultFragment"
        android:name="com.example.umangburman.navdrawerwithnavcomponent.DefaultFragment"
        android:label="Default Fragment"
        tools:layout="@layout/default_fragment" >
    </fragment>

</navigation>
```

### **Step5:** Make up a menu for drawer items:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">

    <group android:checkableBehavior="single">

        <item
            android:id="@+id/first"
            android:icon="@drawable/ic_android_black_24dp"
            android:title="Android" />

        <item
            android:id="@+id/second"
            android:icon="@drawable/ic_attach_money_black_24dp"
            android:title="Dollar" />

        <item
            android:id="@+id/third"
            android:icon="@drawable/ic_save_black_24dp"
            android:title="Save" />

    </group>

</menu>
```

### **Step7:** Change your base theme from AppCompat to MaterialComponents sincxe we are using Androidx:

```xml
<resources>

    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.MaterialComponents.Light.DarkActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
        <item name="windowActionBar">false</item>
        <item name="windowNoTitle">true</item>
    </style>

</resources>
```

### **Step8:** Change your main_activity.xml to:

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <androidx.appcompat.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="@color/colorPrimary"
            android:theme="@style/ThemeOverlay.AppCompat.Dark" />

        <fragment
            android:id="@+id/nav_host_fragment"
            android:name="androidx.navigation.fragment.NavHostFragment"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:defaultNavHost="true"
            app:navGraph="@navigation/nav_graph" />

    </LinearLayout>

    <com.google.android.material.navigation.NavigationView
        android:id="@+id/navigationView"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        android:fitsSystemWindows="true"
        app:headerLayout="@layout/header_layout"
        app:menu="@menu/drawer_menu" />

</androidx.drawerlayout.widget.DrawerLayout>
```

### **Step9:** MainActivity.java:

```Java
import android.os.Bundle;
import android.view.MenuItem;

import com.google.android.material.navigation.NavigationView;

import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.appcompat.widget.Toolbar;
import androidx.core.view.GravityCompat;
import androidx.drawerlayout.widget.DrawerLayout;
import androidx.navigation.NavController;
import androidx.navigation.Navigation;
import androidx.navigation.ui.NavigationUI;

public class MainActivity extends AppCompatActivity implements
                                    NavigationView.OnNavigationItemSelectedListener {
    public Toolbar toolbar;

    public DrawerLayout drawerLayout;

    public NavController navController;

    public NavigationView navigationView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        setupNavigation();

    }

    // Setting Up One Time Navigation
    private void setupNavigation() {

        toolbar = findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
        getSupportActionBar().setDisplayHomeAsUpEnabled(true);
        getSupportActionBar().setDisplayShowHomeEnabled(true);

        drawerLayout = findViewById(R.id.drawer_layout);

        navigationView = findViewById(R.id.navigationView);

        navController = Navigation.findNavController(this, R.id.nav_host_fragment);

        NavigationUI.setupActionBarWithNavController(this, navController, drawerLayout);

        NavigationUI.setupWithNavController(navigationView, navController);

        navigationView.setNavigationItemSelectedListener(this);

    }

    @Override
    public boolean onSupportNavigateUp() {
        return NavigationUI.navigateUp(drawerLayout, Navigation.findNavController(this, R.id.nav_host_fragment));
    }

    @Override
    public void onBackPressed() {
        if (drawerLayout.isDrawerOpen(GravityCompat.START)) {
            drawerLayout.closeDrawer(GravityCompat.START);
        } else {
            super.onBackPressed();
        }
    }

    @Override
    public boolean onNavigationItemSelected(@NonNull MenuItem menuItem) {

        menuItem.setChecked(true);

        drawerLayout.closeDrawers();

        int id = menuItem.getItemId();

        switch (id) {

            case R.id.first:
                navController.navigate(R.id.firstFragment);
                break;

            case R.id.second:
                navController.navigate(R.id.secondFragment);
                break;

            case R.id.third:
                navController.navigate(R.id.thirdFragment);
                break;

        }
        return true;

    }
}
```

### **Finally** Your Fragments should be like this depending on your functionality logic:

```Java
import android.content.Context;
import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;

import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.fragment.app.Fragment;

public class DefaultFragment extends Fragment {

    @Override
    public void onAttach(Context context) {
        super.onAttach(context);
    }

    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        return inflater.inflate(R.layout.default_fragment, container, false);
    }
}
```

For any clarifications please refer to the repository.


## **Conclusion**

The goal of any Navigation Component is to acheive the best possible solution and save development time by eliminating **Fragment Manager** and **Fragment Transaction** class. 

I hope it will help you too.
