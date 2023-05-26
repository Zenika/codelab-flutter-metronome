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

Ouvrez votre terminal et exécutez la commande suivante.

```
flutter pub add audioplayers
```

### Télécharger le son du métronome

Télécharger le fichier MP3 au moyen du lien suivant:

<a href="https://github.com/Zenika/codelab-flutter-metronome/tree/main/assets/metronome-sound.mp3" download>Télécharger le son du métronome</a>

Placez ce fichier dans le dossier `assets` à la racine de votre projet. Le nom du dossier est important. Son nommage est contraint par la librairie `audioplayers` que nous utilisons.

Modifiez ensuite le fichier `pubspec.yaml` afin que Flutter ait connaissance de cette ressource locale. Renseignez dans ce fichier une nouvelle entrée au niveau du paramètre `flutter.assets`:

```yaml
# pubspec.yaml

flutter:
  assets:
    - assets/metronome-sound.mp3 # chemin vers le fichier MP3 précédemment tééchargé
  # ...
# ...
```

### Utiliser InheritedWidget en guise d'injection de dépendance

```dart
// lib/audio_player_provider.dart

import 'package:audioplayers/audioplayers.dart';
import 'package:flutter/material.dart';

class AudioPlayerProvider extends InheritedWidget {
  static AudioPlayerProvider of(BuildContext context) {
    final AudioPlayerProvider? result =
        context.dependOnInheritedWidgetOfExactType<AudioPlayerProvider>();

    if (result == null) {
      throw 'No AudioPlayerProvider found';
    }

    return result;
  }

  static Future<AudioPlayer> createAudioPlayer() async {
    final audioPlayer = AudioPlayer()
      ..setPlayerMode(
        PlayerMode.lowLatency,
      );
    await audioPlayer.setSource(AssetSource('metronome-sound.mp3'));
    return audioPlayer;
  }

  final AudioPlayer audioPlayer;

  const AudioPlayerProvider({
    super.key,
    required super.child,
    required this.audioPlayer,
  });

  @override
  bool updateShouldNotify(AudioPlayerProvider oldWidget) {
    return false;
  }
}
```

### Injecter AudioPlayerProvider dans l'application

```dart
// lib/main.dart

import 'package:flutter/material.dart';
import 'package:metronome/audio_player_provider.dart';
import 'package:metronome/ui/app.dart';

// le point d'entrée de l'application devient asynchone afin que audioplayers charge correctement le son
void main() async {

  // cet appel est désormais nécessaire pour que Flutter s'assure que les widgets soient complètement initialisés
  WidgetsFlutterBinding.ensureInitialized();

  runApp(
    // on place AudioPlayerProvider à la racine de notre application afin que n'importe quel widget de notre application puisse accéder à AudioPlayer
    AudioPlayerProvider(
      audioPlayer: await AudioPlayerProvider.createAudioPlayer(),
      child: const App(),
    ),
  );
}
```

### Jouer le son du métronome

```dart
import 'dart:async';

import 'package:flutter/material.dart';
import 'package:metronome/audio_player_provider.dart';

class RhythmToggleButton extends StatefulWidget {
  const RhythmToggleButton({super.key});

  static Duration getRhythmInterval(int rhythm) =>
      Duration(milliseconds: ((60 / rhythm) * 1000).toInt());

  @override
  State<RhythmToggleButton> createState() => _RhythmToggleButtonState();
}

class _RhythmToggleButtonState extends State<RhythmToggleButton> {
  bool isPlaying = false;
  Timer? periodicTimer;

  @override
  void didChangeDependencies() {
    final audioPlayer = AudioPlayerProvider.of(context).audioPlayer;
    periodicTimer?.cancel();
    periodicTimer = Timer.periodic(
      RhythmToggleButton.getRhythmInterval(50),
      (_) {
        if (isPlaying) {
          audioPlayer.stop();
          audioPlayer.resume();
        }
      },
    );
    super.didChangeDependencies();
  }

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

<aside class="negative">
Comme nous venons de modifier le point d'entrée de notre application et que nous avons ajouté une nouvelle ressource, il est important que vous interrompiez votre application en cours d'exécution, et que vous la redémarriez complètement.
</aside>

Désormais, lorsque vous appuyez sur le bouton "Lecture", le son démarre et il s'interrompt dès lors que vous appuyez sur le bouton "Pause".

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
