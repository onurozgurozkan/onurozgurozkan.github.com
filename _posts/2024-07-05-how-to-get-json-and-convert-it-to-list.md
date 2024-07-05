---
layout: post
title:  "How to get JSON and convert to list in Flutter?"
date:   2024-07-05 06:05:47 +0100
categories: flutter
---

I assume that we have a JSON at `https://webservice.com/dictionary.json`.

```json
[
  {
    "word": "ACD",
    "description": "Automatic Call Distributor"
  },
  {
    "word": "Agent",
    "description": "A person who handles incoming or outgoing customer calls"
  },
  {
    "word": "Analytics",
    "description": "The process of analyzing call data"
  },
]
```

Build a entry. For example at `model/dictonary_entry.dart`

```dart
class DictionaryEntry {
  final String word;
  final String description;

  DictionaryEntry({required this.word, required this.description});

  factory DictionaryEntry.fromJson(Map<String, dynamic> json) {
    return DictionaryEntry(
      word: json['word'],
      description: json['description'],
    );
  }
}
```

Build a service. For example at `service/dictonary_service.dart`

```dart
import 'dart:convert';
import 'package:http/http.dart' as http;
import 'package:dictionary/modal/dictionary_entry.dart';

class DictionaryService {
  static const String _url = 'https://webservice.com/dictionary.json';

  Future<List<DictionaryEntry>> fetchDictionary() async {
    final response = await http.get(Uri.parse(_url));

    if (response.statusCode == 200) {
      List<dynamic> jsonData = json.decode(response.body);
      List<DictionaryEntry> dictionary = jsonData
          .map((entry) => DictionaryEntry.fromJson(entry))
          .toList();
      return dictionary;
    } else {
      throw Exception('Failed to load dictionary');
    }
  }
}
```

## How to use List?

May be try catch is better alternative but this is not topic of this post.

```dart
import 'package:flutter/material.dart';
import 'package:dictionary/service/dictionary_service.dart';

class ListWidget extends StatelessWidget {
  const ListWidget({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: const Text("Call Center Dictonary"),
      ),
      body: Center(
        child: FutureBuilder(
          future: DictionaryService().fetchDictionary(),
          builder: (context, snapshot) {
            if (snapshot.connectionState == ConnectionState.waiting) {
              return const CircularProgressIndicator();
            } else if (snapshot.hasError) {
              return Text('Error: ${snapshot.error}');
            } else if (!snapshot.hasData || snapshot.data!.isEmpty) {
              return const Text('No data found');
            } else {
              return ListView(
                children: snapshot.data!.map((entry) {
                  return ListTile(
                    title: Text(entry.word),
                    subtitle: Text(entry.description),
                  );
                }).toList(),
              );
            }
          },
        ),
      ),
    );
  }
}
```