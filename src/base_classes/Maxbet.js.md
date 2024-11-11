## Maxbet Class Documentation

### Table of Contents

* [**Introduction**](#introduction)
* [**Constructor**](#constructor)
* [**addMaxBet() Method**](#addmaxbet-method)
* [**onMaxBet() Method**](#onmaxbet-method)

### Introduction

The `Maxbet` class is responsible for managing the "Max Bet" button in the game. It handles the display, interaction, and functionality associated with this button.

### Constructor

```javascript
constructor(scene) {
    this.scene = scene;
    this.addMaxBet();
}
```

The constructor initializes the `Maxbet` object by storing a reference to the current scene and calling the `addMaxBet()` method to create and configure the button.

### addMaxBet() Method

```javascript
addMaxBet() {
    this.maxBet = new Sprite(this.scene, Config.width - 477, Config.height - 50, 'bgButtons', 'btn-maxbet.png');
    this.txtMaxBet = this.scene.add.dynamicBitmapText(Config.width - 550, Config.height - 70, 'txt_bitmap', Options.txtMaxBet, 38);
    this.txtMaxBet.setDisplayCallback(this.scene.textCallback);
    this.txtCountMaxBet = this.scene.add.text(Config.width - 555, Config.height - 140, 'BET: ' + Options.coin * Options.line, {
        fontSize: '35px',
        color: '#fff',
        fontFamily: 'PT Serif'
    });
    //pointer down
    this.maxBet.on('pointerdown', this.onMaxbet, this);
    //pointer up
    this.maxBet.on('pointerup', () => this.maxBet.setScale(1));
}
```

The `addMaxBet()` method performs the following actions:

1. **Creates the Max Bet Button:** 
   - Initializes a `Sprite` object representing the button using the provided image and position.
   - Stores this `Sprite` object in the `this.maxBet` property.

2. **Creates Text Display for Max Bet:**
   - Initializes a `dynamicBitmapText` object to display the "Max Bet" label.
   - Stores this `dynamicBitmapText` object in the `this.txtMaxBet` property.
   - Sets the display callback to use the scene's `textCallback` function for text rendering.

3. **Creates Text Display for Current Bet:**
   - Initializes a `text` object to display the current bet amount calculated as `Options.coin * Options.line`.
   - Stores this `text` object in the `this.txtCountMaxBet` property.
   - Configures the text style using the specified font size, color, and font family.

4. **Adds Event Listeners for Interactions:**
   - Attaches a `pointerdown` event listener to the `maxBet` Sprite, calling the `this.onMaxbet` method when the button is pressed.
   - Attaches a `pointerup` event listener to the `maxBet` Sprite, resetting its scale to 1 when the button is released.

### onMaxBet() Method

```javascript
onMaxBet() {
    if (!Options.checkClick && Options.line * Options.coin < 1000 && Options.txtAutoSpin === 'AUTO') {
        this.maxBet.setScale(0.9);
        //play audio button
        this.scene.audioPlayButton();
        Options.line = 20;
        this.scene.btnLine.txtCountLine.setText(Options.line);
        Options.coin = 50;
        this.scene.coin.txtCountCoin.setText(Options.coin);
        this.txtCountMaxBet.setText('BET: ' + Options.line * Options.coin);
    }
}
```

The `onMaxBet()` method handles the logic executed when the Max Bet button is pressed. It performs the following steps:

1. **Check Conditions:**
   - Verifies that clicking is allowed (`!Options.checkClick`).
   - Ensures the current bet amount is less than 1000 (`Options.line * Options.coin < 1000`).
   - Confirms that the auto-spin feature is enabled (`Options.txtAutoSpin === 'AUTO'`).

2. **Update Button Appearance:**
   - Scales the `maxBet` button down slightly (`this.maxBet.setScale(0.9)`), providing visual feedback.

3. **Play Button Sound:**
   - Triggers the scene's `audioPlayButton()` method to play a button click sound.

4. **Set Max Bet Values:**
   - Sets the number of lines to 20 (`Options.line = 20`).
   - Updates the line count display on the line button (`this.scene.btnLine.txtCountLine.setText(Options.line)`).
   - Sets the coin value to 50 (`Options.coin = 50`).
   - Updates the coin count display on the coin button (`this.scene.coin.txtCountCoin.setText(Options.coin)`).
   - Updates the bet amount display on the Max Bet button (`this.txtCountMaxBet.setText('BET: ' + Options.line * Options.coin)`).

**Note:** This method currently implements a simple "max bet" functionality by setting specific line and coin values. It could be further extended to dynamically calculate a maximum bet based on the player's current balance or other game-specific parameters.
