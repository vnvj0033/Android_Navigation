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