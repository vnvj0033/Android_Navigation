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
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:app="http://schemas.android.com/apk/res-auto"
            xmlns:tools="http://schemas.android.com/tools"
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