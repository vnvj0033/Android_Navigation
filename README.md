### Deep Linking
NavDeepLinkBuilder로 Provider에 PendingIntent생성 </br>
navigation Version 2.5.0 이상 사용 권장 (Android 12 PendingIntent 이슈있음)
```kotlin
// DeepLinkAppWidgetProvider.kt
val args = Bundle()
args.putString("myarg", "From Widget")
val pendingIntent = NavDeepLinkBuilder(context)
        .setGraph(R.navigation.mobile_navigation)
        .setDestination(R.id.deeplink_dest)
        .setArguments(args)
        .createPendingIntent()

remoteViews.setOnClickPendingIntent(R.id.deep_link_button, pendingIntent)
```
웹에 연결 ex) www.example.com/myarg
```xml
<!--mobile_navigation.xml-->
<fragment
    android:id="@+id/deeplink_dest"
    android:name="com.example.android.codelabs.navigation.DeepLinkFragment"
    android:label="@string/deeplink"
    tools:layout="@layout/deeplink_fragment">

    <argument
        android:name="myarg"
        android:defaultValue="Android!"/>

    <deepLink app:uri="www.example.com/{myarg}" />
</fragment>
```
```xml
<!--AndroidManifest.xml-->
<activity android:name=".MainActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>

    <nav-graph android:value="@navigation/mobile_navigation" />
</activity>
```

### menus, drawers and bottom navigation
옵션 메뉴
```xml
<!--overflow_menu.xml-->
<item
    android:id="@+id/settings_dest"
    android:icon="@drawable/ic_settings"
    android:menuCategory="secondary"
    android:title="@string/settings" />
```
```kotlin

// MainActivity.kt
override fun onOptionsItemSelected(item: MenuItem): Boolean {
    return item.onNavDestinationSelected(findNavController(R.id.my_nav_host_fragment))
            || super.onOptionsItemSelected(item)
}
```
하단 메뉴
```xml
<!--navigation_activity.xml-->
<com.google.android.material.bottomnavigation.BottomNavigationView
    android:id="@+id/bottom_nav_view"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:menu="@menu/bottom_nav_menu" />
```
```xml
<!--res/menu/bottom_nav_menu.xml-->
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@id/home_dest"
        android:icon="@drawable/ic_home"
        android:title="@string/home" />
    <item
        android:id="@id/deeplink_dest"
        android:icon="@drawable/ic_android"
        android:title="@string/deeplink" />
</menu>
```
```kotlin
//MainActivity.kt
private fun setupBottomNavMenu(navController: NavController) {
    val bottomNav = findViewById<BottomNavigationView>(R.id.bottom_nav_view)
    bottomNav?.setupWithNavController(navController)
}
```
AppBarConfiguration
```kotlin
//MainActivity.kt

// BottomNavigationView가 아닌 NavigationView를 사용
val sideNavView = findViewById<NavigationView>(R.id.nav_view)
sideNavView?.setupWithNavController(navController)

val drawerLayout : DrawerLayout? = findViewById(R.id.drawer_layout)
appBarConfiguration = AppBarConfiguration(
    setOf(R.id.home_dest, R.id.deeplink_dest),
    drawerLayout)

// 대상의 라벨에 따라 ActionBar에 제목 표시
// 최상위 대상이 없을 때마다 위로 버튼 표시
// 최상위 대상이 있을 때 창 아이콘(햄버거 아이콘) 표시
setupActionBarWithNavController(navController, appBarConfig)

override fun onSupportNavigateUp(): Boolean {
    return findNavController(R.id.my_nav_host_fragment).navigateUp(appBarConfiguration)
}
```
```xml
<!--nav_drawer_menu.xml-->
<item
    android:id="@+id/settings_dest"
    android:icon="@drawable/ic_settings"
    android:title="@string/settings" />
```
```xml
<!--navigation_activity.xml-->
<androidx.drawerlayout.widget.DrawerLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.example.android.codelabs.navigation.MainActivity">

    <!--...-->

    <com.google.android.material.navigation.NavigationView
        android:id="@+id/nav_view"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        app:menu="@menu/nav_drawer_menu" />
</androidx.drawerlayout.widget.DrawerLayout>
```



### safe args
safe args를 사용해 Navigation의 안전한 인수 전달 기능
```
// build.gradle
dependencies {
        classpath "androidx.navigation:navigation-safe-args-gradle-plugin:$navigationVersion"
    //...
}

// app/build.gradle
apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
apply plugin: 'androidx.navigation.safeargs.kotlin'
```

```kotlin

// 인수 전달 FlowStepFragment
view.findViewById<Button>(R.id.navigate_action_button)?.setOnClickListener {
    val flowStepNumberArg = 1
    val action = HomeFragmentDirections.nextAction(flowStepNumberArg)
    // HomeFragmentDirections는 navigation의 HomeFragment"
    findNavController().navigate(action)
}

// 인수 수신 HomeFragment
val safeArgs: FlowStepFragmentArgs by navArgs()
val flowStepNumber = safeArgs.flowStepNumber
```

```xml
<!--layout xml-->
<fragment
    android:id="@+id/flow_step_one_dest"
    android:name="com.example.android.codelabs.navigation.FlowStepFragment"
    tools:layout="@layout/flow_step_one_fragment">
    <argument
        android:name="flowStepNumber"
        app:argType="integer"
        android:defaultValue="1"/>

    <action...>
    </action>
</fragment>
```



### Navigate using actions
action을 사용해서 navigation 지원
```xml
<fragment android:id="@+id/home_dest">

    <action android:id="@+id/next_action"
        app:destination="@+id/flow_step_one"
        app:enterAnim="@anim/slide_in_right"
        app:exitAnim="@anim/slide_out_left"
        app:popEnterAnim="@anim/slide_in_left"
        app:popExitAnim="@anim/slide_out_right" />

</fragment>
```


### Navigation Transition
```kotlin
val options = navOptions {
    anim {
        enter = R.anim.slide_in_right
        exit = R.anim.slide_out_left
        popEnter = R.anim.slide_in_left
        popExit = R.anim.slide_out_right
    }
}
view.findViewById<Button>(R.id.navigate_destination_button)?.setOnClickListener {
    findNavController().navigate(R.id.flow_step_one_dest, null, options)
}
```

### NavController
NavController는 Navigate 명령 (NavHostFragment의 Fragment 교체)
```
// NavController을 가저오는 방법
Fragment.findNavController()
View.findNavController()
Activity.findNavController(viewId: Int)
```

```kotlin
// NavController을 사용해 navigate
val button = view.findViewById<Button>(R.id.navigate_destination_button)
button?.setOnClickListener {
    findNavController().navigate(R.id.flow_step_one_dest, null)
}
//or
val button = view.findViewById<Button>(R.id.navigate_destination_button)
button?.setOnClickListener(
    Navigation.createNavigateOnClickListener(R.id.flow_step_one_dest, null)
)
```


### NavHostFragment
NavHostFragment는 Navigate 마다 Fragment로 교체<br/>
activity xml에 fragment의 android:name로 선언<br/>
app:navGraph는 NavHostFragment를 Navigation Graph와 연결

```xml
<LinearLayout>
    <fragment
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:id="@+id/my_nav_host_fragment"
        android:name="androidx.navigation.fragment.NavHostFragment"
        app:navGraph="@navigation/mobile_navigation"
        app:defaultNavHost="true"
    />
</LinearLayout>
```


### Add a Destination

```xml

<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    app:startDestination="@+id/home_dest">
    <!--...-->

    <fragment
        android:id="@+id/settings_dest"
        android:name="com.example.android.codelabs.navigation.SettingsFragment"
        android:label="@string/settings"
        tools:layout="@layout/settings_fragment" />

    <!--...-->
</navigation>
```


### Navigation Graph
```
destinations: 앱에서 이동할 수 있는 모든 위치, 일반적으로 Fragment나 Activity
Navigation Graph : destinations의 경로를 정의하는 리소스 유형
```
navigation xml
<navigation>에는 <activity> 또는 <fragment> 요소로 표시된 대상이 하나 이상 포함</br>
app:startDestination은 앱을 처음 열 때 실행되는 대상
```xml
<navigation 
    xmlns:app="http://schemas.android.com/apk/res-auto"
    app:startDestination="@+id/home_dest">

    <!-- ...tags for fragments and activities here -->
</navigation>
```




# Android Navigation codelab

Content: https://codelabs.developers.google.com/codelabs/android-navigation/

License
-------

Copyright 2018 The Android Open Source Project, Inc.

Licensed to the Apache Software Foundation (ASF) under one or more contributor
license agreements.  See the NOTICE file distributed with this work for
additional information regarding copyright ownership.  The ASF licenses this
file to you under the Apache License, Version 2.0 (the "License"); you may not
use this file except in compliance with the License.  You may obtain a copy of
the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  See the
License for the specific language governing permissions and limitations under
the License.
