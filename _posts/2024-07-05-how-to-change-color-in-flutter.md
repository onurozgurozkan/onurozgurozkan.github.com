---
layout: post
title:  "How to Use Hex Colors in Flutter"
date:   2024-07-05 06:04:47 +0100
categories: flutter
---

How can you use hex colors in Flutter?

For instance, Hipcall’s default color is `025196`. To use this hex color in Flutter, prefix it with `0xFF` and then use it like this: `Color(0xFF025196)`.

```dart
theme: ThemeData(
  colorScheme: ColorScheme.fromSeed(seedColor: Color(0xFF025196)),
  useMaterial3: true,
),
```