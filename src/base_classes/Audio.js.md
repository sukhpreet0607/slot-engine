## Audio Class Documentation

### Table of Contents

| Section | Description |
|---|---|
| [Audio Class](#audio-class) | Overview of the Audio Class |
| [Constructor](#constructor) | Initialization of the Audio class |
| [loadAudio() Method](#loadaudio-method) | Loading and configuring audio assets |

### Audio Class

The `Audio` class is responsible for managing all audio assets used in the game. It handles loading, configuring, and playing sounds.

### Constructor

The constructor initializes the `Audio` class with a reference to the current scene. It also calls the `loadAudio()` method to load all required audio assets.

```javascript
//Class Audio
export default class Audio {
    constructor(scene) {
        this.scene = scene;
        this.loadAudio();
    }

    // ...
}
```

### loadAudio() Method

The `loadAudio()` method loads and configures all audio assets used in the game. The method uses the Phaser `scene.sound.add()` method to add each sound to the scene's sound manager. It also sets specific properties for each sound, such as `loop`, `volume`, etc.

```javascript
    loadAudio() {
        // Loads and configures background music
        this.musicBackgroundDefault = this.scene.sound.add('backgroundDefault', {
            loop: true, // Sets the music to loop continuously
            volume: 1.5 // Sets the music volume to 1.5
        });

        // Loads and configures sound for reels spinning
        this.audioReels = this.scene.sound.add('reels');

        // Loads and configures sound for reels stopping
        this.audioReelStop = this.scene.sound.add('reelStop');

        // Loads and configures sound for winning
        this.audioWin = this.scene.sound.add('win', { loop : true }); // Sets the win sound to loop continuously

        // Loads and configures sound for button clicks
        this.audioButton = this.scene.sound.add('button');

        // Loads and configures sound for losing
        this.audioLose = this.scene.sound.add('lose', { volume: 2.5 }); // Sets the losing sound volume to 2.5

        // Loads and configures default music
        this.musicDefault = this.scene.sound.add('musicDefault', { 
            loop: true,
            volume: 2
        });
    }
```

This method demonstrates a common approach for managing audio assets in Phaser games. It provides a clear and organized way to load and configure all necessary sounds, making them easily accessible for later use in the game. 
