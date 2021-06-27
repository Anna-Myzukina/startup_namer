# startup_namer

A new Flutter project.

## Getting Started

This project is a starting point for a Flutter application.

A few resources to get you started if this is your first Flutter project:

- [Lab: Write your first Flutter app](https://flutter.dev/docs/get-started/codelab)
- [Cookbook: Useful Flutter samples](https://flutter.dev/docs/cookbook)

For help getting started with Flutter, view our
[online documentation](https://flutter.dev/docs), which offers tutorials,
samples, guidance on mobile development, and a full API reference.
# startup_namer


## Let`s starting the app:

- [x] Create a Flutter project called startup_namer and migrate to null safety as follows.


        $ flutter create startup_namer
        $ cd startup_namer
        $ dart migrate --apply-changes

You'll mostly edit lib/main.dart, where the Dart code lives.

- [X] Replace it with the code from this file, which displays an infinite, lazily loaded list of proposed startup names.

        // Copyright 2018 The Flutter team. All rights reserved.
        // Use of this source code is governed by a BSD-style license that can be
        // found in the LICENSE file.

        import 'package:english_words/english_words.dart';
        import 'package:flutter/material.dart';

        void main() => runApp(MyApp());

        // #docregion MyApp
        class MyApp extends StatelessWidget {
        // #docregion build
        @override
        Widget build(BuildContext context) {
            return MaterialApp(
            title: 'Startup Name Generator',
            home: RandomWords(),
            );
        }
        // #enddocregion build
        }
        // #enddocregion MyApp

        // #docregion RWS-var
        class _RandomWordsState extends State<RandomWords> {
        final _suggestions = <WordPair>[];
        final _biggerFont = const TextStyle(fontSize: 18.0);
        // #enddocregion RWS-var

        // #docregion _buildSuggestions
        Widget _buildSuggestions() {
            return ListView.builder(
                padding: const EdgeInsets.all(16.0),
                itemBuilder: /*1*/ (context, i) {
                if (i.isOdd) return const Divider(); /*2*/

                final index = i ~/ 2; /*3*/
                if (index >= _suggestions.length) {
                    _suggestions.addAll(generateWordPairs().take(10)); /*4*/
                }
                return _buildRow(_suggestions[index]);
                });
        }
        // #enddocregion _buildSuggestions

        // #docregion _buildRow
        Widget _buildRow(WordPair pair) {
            return ListTile(
            title: Text(
                pair.asPascalCase,
                style: _biggerFont,
            ),
            );
        }
        // #enddocregion _buildRow

        // #docregion RWS-build
        @override
        Widget build(BuildContext context) {
            return Scaffold(
            appBar: AppBar(
                title: const Text('Startup Name Generator'),
            ),
            body: _buildSuggestions(),
            );
        }
        // #enddocregion RWS-build
        // #docregion RWS-var
        }
        // #enddocregion RWS-var

        class RandomWords extends StatefulWidget {
        @override
        State<RandomWords> createState() => _RandomWordsState();
        }

- [x] Update pubspec.yaml by adding the English words package:


        dependencies:
          flutter:
            sdk: flutter

          cupertino_icons: ^1.0.2
          english_words: ^4.0.0      // NEW

The English words package generates pairs of random words, which are used as potential startup names.

While viewing the pubspec in Android Studio's editor view, click Pub get in the upper right, which pulls the package into your project. You should see the following in the console:


    flutter pub get
    Running "flutter pub get" in startup_namer...
    Process finished with exit code 0
    
Run the app.


