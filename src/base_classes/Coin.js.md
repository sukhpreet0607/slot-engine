## Coin Class Documentation

### Table of Contents

* [1. Introduction](#1-introduction)
* [2. Constructor](#2-constructor)
* [3. addCoin() Method](#3-addcoin-method)
* [4. onCoin() Method](#4-oncoin-method)

### 1. Introduction

The `Coin` class represents the coin element in the game, responsible for managing the player's coin balance and handling coin-related actions.

### 2. Constructor

The `constructor` initializes a new `Coin` object.

| Parameter | Type | Description |
|---|---|---|
| `scene` | Object | The scene where the coin is added. |

**Code:**

```javascript
constructor(scene) {
    this.scene = scene;
    this.addCoin();
}
```

**Explanation:**

- The constructor takes the `scene` as an argument and stores it in the `this.scene` property.
- It calls the `addCoin()` method to create and initialize the coin element.

### 3. addCoin() Method

The `addCoin()` method creates the visual representation of the coin, the text displaying the coin balance, and sets up event listeners for interaction.

**Code:**

```javascript
addCoin() {
    this.coin = new Sprite(this.scene, Config.width - 678, Config.height - 50, 'bgButtons', 'btn-coin.png');
    this.txtCoin = this.scene.add.dynamicBitmapText(Config.width - 720, Config.height - 70, 'txt_bitmap', Options.txtCoin, 38);
    this.txtCoin.setDisplayCallback(this.scene.textCallback);
    this.txtCountCoin = this.scene.add.text(Config.width - 700, Config.height - 140, Options.coin, {
        fontSize : '35px',
        color : '#fff',
        fontFamily : 'PT Serif'
    });
    //pointer down
    this.coin.on('pointerdown', this.onCoin, this);
    //pointer up
    this.coin.on('pointerup', () => this.coin.setScale(1));
}
```

**Explanation:**

1. **Coin Sprite:**
   - A new `Sprite` object is created using the `Sprite` class.
   - It is positioned at `Config.width - 678` and `Config.height - 50`.
   - The `'bgButtons'` key is used to access the `'btn-coin.png'` image from the game's assets.

2. **Coin Balance Text:**
   - A dynamic `BitmapText` object is created using `this.scene.add.dynamicBitmapText()`.
   - It is positioned at `Config.width - 720` and `Config.height - 70`.
   - The `'txt_bitmap'` key is used to access the bitmap font from the assets.
   - `Options.txtCoin` is the initial text displayed on the coin.
   - `38` specifies the font size.
   - `this.scene.textCallback` is a callback function that handles the display of text.

3. **Coin Count Text:**
   - A `Text` object is created using `this.scene.add.text()`.
   - It is positioned at `Config.width - 700` and `Config.height - 140`.
   - `Options.coin` is the initial coin count displayed.
   - The `fontSize`, `color`, and `fontFamily` properties are set to style the text.

4. **Event Listeners:**
   - A `pointerdown` event listener is added to the `this.coin` sprite, which calls the `this.onCoin()` method when the coin is clicked.
   - A `pointerup` event listener is added to the `this.coin` sprite, which resets the scale of the coin to `1` when the click is released.

### 4. onCoin() Method

The `onCoin()` method handles the logic when the coin is clicked. It increases the player's coin balance by 10, updates the displayed coin count, and resets the balance if it reaches 50.

**Code:**

```javascript
onCoin() {
    if (!Options.checkClick && Options.txtAutoSpin === 'AUTO') {
        this.coin.setScale(0.9);
        //play audio button
        this.scene.audioPlayButton();
        if (Options.coin < 50) {
            Options.coin += 10;
            this.txtCountCoin.setText(Options.coin);
            this.scene.maxBet.txtCountMaxBet.setText('BET: ' + Options.coin * Options.line);
        } else {
            Options.coin = 10;
            this.txtCountCoin.setText(Options.coin);
            this.scene.maxBet.txtCountMaxBet.setText('BET: ' + Options.coin * Options.line);
        }
    }
}
```

**Explanation:**

1. **Check for Click and Auto Spin:**
   - The function first checks if `Options.checkClick` is false and `Options.txtAutoSpin` is equal to 'AUTO'. This ensures that the coin can only be clicked when clicks are enabled and the game is not in auto-spin mode.

2. **Coin Scaling:**
   - If the conditions in step 1 are met, the scale of the coin sprite is reduced to 0.9, providing visual feedback to the player.

3. **Play Audio:**
   - The `this.scene.audioPlayButton()` method is called to play a sound effect when the coin is clicked.

4. **Increase Coin Balance:**
   - If the current `Options.coin` is less than 50, it is increased by 10.
   - The text of `this.txtCountCoin` is updated to display the new coin count.
   - The text of `this.scene.maxBet.txtCountMaxBet` is updated to display the current bet amount, which is calculated as `Options.coin * Options.line`.

5. **Reset Coin Balance:**
   - If the current `Options.coin` is 50 or more, it is reset to 10.
   - The text of `this.txtCountCoin` and `this.scene.maxBet.txtCountMaxBet` is updated to reflect the new balance and bet amount.
