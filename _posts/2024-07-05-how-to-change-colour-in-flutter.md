---
layout: post
title:  "How to use hex colours in Flutter"
date:   2024-07-05 06:04:47 +0100
categories: flutter
---

For instance, [Hipcall](https://www.hipcall.com/en-us/brand-and-design-guide/)’s 
default color is `025196`. To use this hex color in Flutter, prefix it with 
`0xFF` and then use it like this: `Color(0xFF025196)`.

```dart
theme: ThemeData(
  colorScheme: ColorScheme.fromSeed(seedColor: Color(0xFF025196)),
  useMaterial3: true,
),
```