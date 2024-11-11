## BaseSpin Class Documentation

### Table of Contents

* [BaseSpin Class](#basespin-class)
    * [Constructor](#constructor)
    * [addSpin()](#addspin)
    * [playTweens()](#playtweens)
    * [destroyLineArr()](#destroylinear)
    * [removeTextWin()](#removetextwin)
    * [setColor()](#setcolor)
    * [saveLocalStorage()](#savelocalstorage)

### BaseSpin Class

This class represents the spin button functionality in the game. It handles the visual elements of the spin button, manages user interaction, and triggers game logic upon clicking the button.

#### Constructor

```javascript
constructor(scene) {
  this.scene = scene;
  this.addSpin();
}
```

The constructor initializes the `BaseSpin` instance with a reference to the `scene` where it belongs. It calls the `addSpin()` method to create the visual components of the spin button.

#### addSpin()

```javascript
addSpin() {
  // Create the background image for the spin button.
  this.bgSpin = new Sprite(this.scene, Config.width - 275, Config.height - 50, 'bgButtons', 'btn-spin.png');

  // Create the text label for the spin button.
  this.txtSpin = this.scene.add.dynamicBitmapText(Config.width - 315, Config.height - 70, 'txt_bitmap', Options.txtSpin, 38);
  // Set the display callback for the bitmap text.
  this.txtSpin.setDisplayCallback(this.scene.textCallback);

  // Add event listeners for pointer interaction with the spin button.
  this.bgSpin.on('pointerdown', this.playTweens, this);
  this.bgSpin.on('pointerup', () => this.bgSpin.setScale(1));
}
```

This method creates the visual elements of the spin button:

* **`this.bgSpin`:** A sprite object representing the background image of the button.
* **`this.txtSpin`:** A `dynamicBitmapText` object displaying the text "SPIN" on the button.

It also adds event listeners for pointer interaction:

* **`pointerdown`:** Triggers the `playTweens()` method when the button is pressed.
* **`pointerup`:** Resets the scale of the button to 1 when the pointer is released.

#### playTweens()

```javascript
playTweens() {
  if (!Options.checkClick && this.scene.valueMoney >= (Options.coin * Options.line) && Options.txtAutoSpin === 'AUTO') {
    // Destroy the line array (resets lines on the reels).
    this.destroyLineArr();

    // Set the tint for the spin button and related elements.
    this.setColor();

    // Set the click flag to true.
    Options.checkClick = true;

    // Scale down the button.
    this.bgSpin.setScale(0.9);

    // Remove the winning text from the screen.
    this.removeTextWin();

    // Save the current money value to localStorage.
    this.saveLocalStorage();

    // Create a new Tween instance to handle animation.
    this.tweens = new Tween(this.scene);
  }
}
```

This method is called when the spin button is pressed. It performs the following actions:

* **Checks conditions:**
    * **`!Options.checkClick`:** Ensures the button is not already pressed.
    * **`this.scene.valueMoney >= (Options.coin * Options.line)`:** Verifies enough money is available to spin.
    * **`Options.txtAutoSpin === 'AUTO'`:** Checks if auto-spin is enabled.

* **If all conditions are met:**
    * **`destroyLineArr()`:** Removes the previous lines on the reels.
    * **`setColor()`:** Tints the button and other elements.
    * **`Options.checkClick = true`:** Prevents further clicks until the spin is complete.
    * **`this.bgSpin.setScale(0.9)`:** Scales down the button for visual feedback.
    * **`removeTextWin()`:** Clears any previous winning text.
    * **`saveLocalStorage()`:** Saves the current money value.
    * **`this.tweens = new Tween(this.scene)`:** Creates a new `Tween` instance to handle the spin animation.

#### destroyLineArr()

```javascript
destroyLineArr() {
  if (Options.lineArray.length > 0) {
    for (let i = 0; i < Options.lineArray.length; i++) {
      Options.lineArray[i].destroy();
    }
    Options.lineArray = [];
  }
}
```

This method clears the `Options.lineArray` which holds references to the lines displayed on the reels. It iterates through the array and destroys each line object.

#### removeTextWin()

```javascript
removeTextWin() {
  // Play the button sound effect.
  this.scene.audioPlayButton();

  if (this.scene.audioMusicName === 'btn_music.png') {
    // Stop the winning audio if it's playing.
    this.scene.audioObject.audioWin.stop();

    // Play the reel audio.
    this.scene.audioObject.audioReels.play();
  }

  // Subtract the bet amount from the player's money.
  this.scene.valueMoney -= (Options.coin * Options.line);

  // Update the displayed money value.
  this.scene.txtMoney.setText(this.scene.valueMoney + '$');

  // Destroy any existing winning text.
  if (this.scene.txtWin) {
    this.scene.txtWin.destroy();
  }
}
```

This method handles the audio and money management after a spin is triggered:

* **Play the button sound effect.**
* **Stop the winning audio and play the reel audio if needed.**
* **Subtract the bet amount from the player's money.**
* **Update the displayed money value.**
* **Destroy any existing winning text.**

#### setColor()

```javascript
setColor() {
  this.bgSpin.setTint(0xa09d9d);
  this.scene.autoSpin.buttonAuto.setTint(0xa09d9d);
  this.scene.maxBet.maxBet.setTint(0xa09d9d);
  this.scene.coin.coin.setTint(0xa09d9d);
  this.scene.btnLine.btnLine.setTint(0xa09d9d);
  this.scene.btnMusic.setTint(0xa09d9d);
  this.scene.btnSound.setTint(0xa09d9d);
}
```

This method sets a specific tint color (0xa09d9d) to the spin button and several other UI elements.

#### saveLocalStorage()

```javascript
saveLocalStorage() {
  if (localStorage.getItem('money')) {
    localStorage.removeItem('money');
    localStorage.setItem('money', this.scene.valueMoney);
  }
  localStorage.setItem('money', this.scene.valueMoney);
  this.scene.setTextX(this.scene.valueMoney);
  this.scene.txtMoney.setText(this.scene.valueMoney + '$');
}
```

This method saves the player's current money value to localStorage. It first checks if a previous money value exists in localStorage and removes it before saving the new value. It also updates the text display of the player's money. 
