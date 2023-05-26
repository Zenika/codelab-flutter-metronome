summary: Dans ce codelab, vous allez apprendre à développer une application Flutter en réalisant un métronome
id: docs
categories: Flutter
tags: beginner
status: Published
authors: Jérémy Riverain
Feedback Link: https://github.com/Zenika/codelab-flutter-metronome/issues

# Développer une application Flutter

<!-- ------------------------ -->

## Introduction

Duration: 2

![Logo Flutter](./assets/flutter-logo.png)

### Ce que vous allez réaliser

<img src="./assets/layout_phone.png" width="300">

<!-- ------------------------ -->

## Configurer l’environnement

Duration: 2

```
flutter doctor
flutter create metronome
cd metronome
rm lib/main.dart
rm -rf test
```

<!-- ------------------------ -->

## Ajouter les premiers composants structurants

### Afficher "Hello Flutter"

```dart
// lib/main.dart

import 'package:flutter/material.dart';
import 'package:metronome/app.dart';

void main() {
  runApp(const App());
}
```

```dart
// lib/ui/app.dart

import 'package:flutter/material.dart';

class App extends StatelessWidget {
  const App({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Center(
          child: Text(
            'Hello Flutter',
            style: Theme.of(context).textTheme.displayMedium,
          ),
        ),
      ),
    );
  }
}
```

### Afficher le rythme du métronome

```dart
// lib/ui/rhythm_label.dart

import 'package:flutter/material.dart';

class RhythmLabel extends StatelessWidget {
  const RhythmLabel({super.key});
  @override
  Widget build(BuildContext context) {
    return Text(
      '50',
      style: Theme.of(context).textTheme.displayLarge?.copyWith(
            fontSize: 80,
            fontWeight: FontWeight.bold,
          ),
    );
  }
}
```

```dart
// lib/ui/app.dart

import 'package:flutter/material.dart';
import 'package:metronome/ui/rhythm_label.dart'; // importer RhythmLabel

class App extends StatelessWidget {
  const App({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: RhythmLabel(), // remplacer le texte précédent par RhythmLabel
      ),
    );
  }
}
```

### Afficher le "slider" permettant de modifier le rythme

```dart
// lib/ui/rhythm_slider.dart

import 'package:flutter/material.dart';

class RhythmSlider extends StatelessWidget {
  const RhythmSlider({super.key});
  @override
  Widget build(BuildContext context) {
    return Slider(
      value: 50,
      min: 30,
      max: 200,
      onChanged: (value) {},
    );
  }
}
```

```dart
// lib/ui/app.dart
import 'package:flutter/material.dart';
import 'package:metronome/ui/rhythm_label.dart';
import 'package:metronome/ui/rhythm_slider.dart'; // importer RhythmSlider

class App extends StatelessWidget {
  const App({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Column(
          // utiliser Column pour afficher plusieurs widgets dans une colonne
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            RhythmLabel(),
            Padding( // ajouter un peu d'espacement en haut et en bas du slider
              padding: const EdgeInsets.symmetric(vertical: 8),
              child: RhythmSlider(), // utiliser RhythmSlider
            ),
          ],
        ),
      ),
    );
  }
}
```

### Afficher le bouton permettant de démarrer/arrêter le métronome

```dart
// lib/ui/rhythm_toggle_button.dart

import 'package:flutter/material.dart';

class RhythmToggleButton extends StatefulWidget {
  const RhythmToggleButton({super.key});

  @override
  State<RhythmToggleButton> createState() => _RhythmToggleButtonState();
}

class _RhythmToggleButtonState extends State<RhythmToggleButton> {
  bool isPlaying = false;
  @override
  Widget build(BuildContext context) {
    return TextButton(
      onPressed: () {
        setState(() {
          isPlaying = !isPlaying;
        });
      },
      child: Icon(
        isPlaying ? Icons.pause : Icons.play_arrow,
        size: 120,
      ),
    );
  }
}
```

```dart
// lib/ui/app.dart

import 'package:flutter/material.dart';
import 'package:metronome/ui/rhythm_label.dart';
import 'package:metronome/ui/rhythm_slider.dart';
import 'package:metronome/ui/rhythm_toggle_button.dart'; // importer RhythmToggleButton

class App extends StatelessWidget {
  const App({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            RhythmLabel(),
            Padding(
              padding: const EdgeInsets.symmetric(vertical: 8),
              child: RhythmSlider(),
            ),
            RhythmToggleButton(), // utiliser RhythmToggleButton
          ],
        ),
      ),
    );
  }
}
```

### Ajuster le thème général

```dart
// lib/ui/app.dart

import 'package:flutter/material.dart';
import 'package:metronome/ui/rhythm_label.dart';
import 'package:metronome/ui/rhythm_slider.dart';
import 'package:metronome/ui/rhythm_toggle_button.dart'; // importer RhythmToggleButton

class App extends StatelessWidget {
  const App({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      theme: ThemeData(
        useMaterial3: true, // activation de Material Design 3
      ),
      debugShowCheckedModeBanner: false, // dissimule la bannière "debug"
      home: Scaffold(
        body: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            RhythmLabel(),
            Padding(
              padding: const EdgeInsets.symmetric(vertical: 8),
              child: RhythmSlider(),
            ),
            RhythmToggleButton(), // utiliser RhythmToggleButton
          ],
        ),
      ),
    );
  }
}
```

<img src="./assets/layout_phone.png" width="300">

<!-- ------------------------ -->

## Rendre l’application responsive

Duration: 2

### Utiliser le widget LayoutBuilder

```dart
// lib/ui/app.dart

import 'package:flutter/material.dart';
import 'package:metronome/ui/rhythm_label.dart';
import 'package:metronome/ui/rhythm_slider.dart';
import 'package:metronome/ui/rhythm_toggle_button.dart';

class App extends StatelessWidget {
  const App({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      theme: ThemeData(
        useMaterial3: true,
      ),
      debugShowCheckedModeBanner: false,
      home: Scaffold(
        body: LayoutBuilder(builder: (context, constraints) {
          if (constraints.maxWidth > 500) { // Mise en page grand écran
            return const Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                SizedBox(
                  width: 300,
                  child: Column(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: [
                      RhythmLabel(),
                      Padding(
                        padding: EdgeInsets.only(top: 8),
                        child: RhythmSlider(),
                      ),
                    ],
                  ),
                ),
                RhythmToggleButton(),
              ],
            );
          }

          // Mise en page petit écran (téléphone)
          return const Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              RhythmLabel(),
              Padding(
                padding: EdgeInsets.symmetric(vertical: 8),
                child: RhythmSlider(),
              ),
              RhythmToggleButton(),
            ],
          );
        }),
      ),
    );
  }
}
```

<!-- ------------------------ -->

## Jouer un son à intervalle régulier

Duration: 2

L'enjeu de cette section est de jouer le son du métronome à intervalle régulier.

J'imagine qu'il serait possible de coder le lancement du son en pur Dart. Ceci dit, pour se faciliter la tâche, nous allons plutôt installer une librairie.

Pour choisir une librairie, 2 sites sont à connaître:

- l'incontournable [pub.dev](https://pub.dev/): c'est le site officiel répertoriant tous les paquets Dart et Flutter
- [fluttergems](https://fluttergems.dev/): j'apprécie ce site pour explorer le champ des possibles, sans trop savoir quel sera la librairie de mon choix. L'UI et les critères de recherche de ce site me paraissent plus intuitifs que le site officiel [pub.dev](https://pub.dev/). Je le recommande donc si vous ne savez pas trop quoi chercher.

Pour jouer le son du métronome, je jette mon dévolu sur la librairie [audioplayers](https://github.com/bluefireteam/audioplayers) que j'ai déjà utilisé sur un projet [Flame](https://docs.flame-engine.org/latest/) pour réaliser un jeu 2D avec Flutter. Cette librairie va nous permettre de facilement jouer un son provenant de nos ressources locales.

![Fluttergems](./assets/fluttergems.png)

<aside class="positive">
<div>
  <img src="./assets/flame.png" width="100" alt="logo flame" />
</div>

Si d'aventure vous vouliez créer un jeu 2D, Flutter est un très candidat grâce au moteur de jeu <a href="https://docs.flame-engine.org/latest/">Flame</a>.

</aside>

### Installer audioplayers

```
flutter pub add audioplayers:^4.0.0
```

### Télécharger le son du métronome

<a href="https://github.com/Zenika/codelab-flutter-metronome/tree/main/assets/metronome-sound.mp3" download>Télécharger le son du métronome</a>

<!-- ------------------------ -->

## Modifier le rythme du métronome

Duration: 2

TODO

<!-- ------------------------ -->

## Tester les fonctionnalités du métronome

Duration: 2

TODO

<!-- ------------------------ -->

## Exécuter les tests en CI

Duration: 2

TODO

<!-- ------------------------ -->

## Félicitations

Duration: 2

TODO
