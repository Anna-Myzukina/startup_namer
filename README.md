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

![img](https://github.com/Anna-Myzukina/startup_namer/blob/main/screenschot/screen1.PNG)

##  Now Add icons to the list:
In this step, you'll add heart icons to each row. In the next step, you'll make them tappable and save the favorites.

- [x] Add a _saved Set to _RandomWordsState. This Set stores the word pairings that the user favorited. Set is preferred to List because a properly implemented Set doesn't allow duplicate entries:

        class _RandomWordsState extends State<RandomWords> {
          final _suggestions = <WordPair>[];
          final _saved = <WordPair>{};     // NEW
          final _biggerFont = TextStyle(fontSize: 18.0);
          ...
        }
        
 - [x] In the _buildRow function, add an alreadySaved check to ensure that a word pairing has not already been added to favorites:
  
          Widget _buildRow(WordPair pair) {
          final alreadySaved = _saved.contains(pair);  // NEW
          ...
        }
        
- [x]  In _buildRow() , you'll also add heart-shaped icons to the ListTile objects to enable favoriting. In the next step, you'll add the ability to interact with the heart icons.

Add the icons after the text, as shown below:


        Widget _buildRow(WordPair pair) {
          final alreadySaved = _saved.contains(pair);
          return ListTile(
            title: Text(
              pair.asPascalCase,
              style: _biggerFont,
            ),
            trailing: Icon(   // NEW from here... 
              alreadySaved ? Icons.favorite : Icons.favorite_border,
              color: alreadySaved ? Colors.red : null,
            ),                // ... to here.
          );
        }


You should now see open hearts on each row, but they are not yet interactive.

![img](https://github.com/Anna-Myzukina/startup_namer/blob/main/screenschot/screen2.PNG)


## Add interactivity
In this step, you'll make the heart icons tappable. When the user taps an entry in the list, toggling its favorited state, that word pairing is added or removed from a set of saved favorites.

- [x] To do that, you'll modify the _buildRow function. If a word entry has already been added to favorites, tapping it again removes it from favorites. When a tile has been tapped, the function calls setState() to notify the framework that state has changed.

Add onTap to the _buildRow method, as shown below:

        Widget _buildRow(WordPair pair) {
          final alreadySaved = _saved.contains(pair);
          return ListTile(
            title: Text(
              pair.asPascalCase,
              style: _biggerFont,
            ),
            trailing: Icon(
              alreadySaved ? Icons.favorite : Icons.favorite_border,
              color: alreadySaved ? Colors.red : null,
            ),
            onTap: () {      // NEW lines from here...
              setState(() {
                if (alreadySaved) {
                  _saved.remove(pair);
                } else { 
                  _saved.add(pair); 
                } 
              });
            },               // ... to here.
          );
        }
        
        
Hot reload the app.

You should be able to tap any tile to favorite or unfavorite the entry. Tapping a tile generates an implicit ink splash animation emanating from the tap point.

![img](https://github.com/Anna-Myzukina/startup_namer/blob/main/screenschot/screen3.PNG)

