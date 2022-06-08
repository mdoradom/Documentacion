---
title: Android Cheat Sheet
author: mario
date: 2022-06-08 16:23:00 +0800
categories: [Android]
tags: [cheatsheet, android]
math: true
mermaid: true
---

Android Cheat Sheet
========================

## ViewBinding

``build.gradle`` 

```java
android {
    buildFeatures {
        viewBinding true
    }
}
```

``Activity.java``

```java
setContentView((binding = ActivityMainBinding.inflate(getLayoutInflater())).getRoot());
```

``Fragment.java``

```java
return (binding = FragmentMainBinding.inflate(inflater, container, false)).getRoot();
```

## Navigation

```java
<androidx.fragment.app.FragmentContainerView
    android:name="androidx.navigation.fragment.NavHostFragment"
    android:id="@+id/nav_host_fragment"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:defaultNavHost="true"
    app:navGraph="@navigation/nav_graph" />
```

``NavController (Fragment)``

```java
navController = Navigation.findNavController(view);
```

``NavController (Activity)``

```java
navController = ((NavHostFragment) getSupportFragmentManager().findFragmentById(R.id.nav_host_fragment)).getNavController();
```

``NavController (DialogFragment)``

```java
navController = Navigation.findNavController(requireParentView());
```