## Line Class Documentation

### Table of Contents

* [1. Introduction](#1-introduction)
* [2. Constructor](#2-constructor)
* [3. addLine Method](#3-addline-method)

### 1. Introduction

The `Line` class is responsible for creating and managing the line bet button and its associated text elements. It utilizes the `Sprite` class to create the button image and `dynamicBitmapText` and `text` classes to display the line count and bet amount.

### 2. Constructor

The `Line` constructor initializes the class with a reference to the scene it belongs to. It then calls the `addLine()` method to create and configure the line bet button and text elements.

```javascript
//Class Line
export default class Line {
    constructor(scene) {
        this.scene = scene;
        this.addLine();
    }
}
```

### 3. addLine Method

The `addLine` method creates the following elements:

* **btnLine:** A `Sprite` object representing the line bet button. It is positioned at `Config.width - 865, Config.height - 50` with the image `'btn-line.png'` from the `'bgButtons'` spritesheet.
* **txtLine:** A `dynamicBitmapText` object displaying the text `Options.txtLine` in the `'txt_bitmap'` font style. It is positioned at `Config.width - 915, Config.height - 70` with a font size of 38.
* **txtCountLine:** A `text` object displaying the current line count (`Options.line`). It is positioned at `Config.width - 880, Config.height - 140` with a font size of 35px, white color, and the `'PT Serif'` font family.

The method then adds event listeners to the `btnLine` for `pointerdown` and `pointerup` events.

```javascript
addLine() {
    this.btnLine = new Sprite(this.scene, Config.width - 865, Config.height - 50, 'bgButtons', 'btn-line.png');
    this.txtLine = this.scene.add.dynamicBitmapText(Config.width - 915, Config.height - 70, 'txt_bitmap', Options.txtLine, 38);
    this.txtLine.setDisplayCallback(this.scene.textCallback);
    this.txtCountLine = this.scene.add.text(Config.width - 880, Config.height - 140, Options.line, {
        fontSize : '35px',
        color : '#fff',
        fontFamily : 'PT Serif'
    });

    //pointer down
    this.btnLine.on('pointerdown', () => {
        if (!Options.checkClick && Options.txtAutoSpin === 'AUTO') {
            this.btnLine.setScale(0.9);
            //play audio button
            this.scene.audioPlayButton();

            if (Options.line < 20) {
                Options.line ++;
                this.txtCountLine.setText(Options.line);
                this.scene.maxBet.txtCountMaxBet.setText('BET: ' + Options.line * Options.coin);
            } else {
                Options.line = 1;
                this.txtCountLine.setText(Options.line);
                this.scene.maxBet.txtCountMaxBet.setText('BET: ' + Options.line * Options.coin);
            }
        }
    });

    //pointer up
    this.btnLine.on('pointerup', () => this.btnLine.setScale(1));
}
```

**Pointer Down Event:**

* The `pointerdown` event listener checks if the `Options.checkClick` flag is false and the `Options.txtAutoSpin` value is 'AUTO'. This ensures that the button is only clickable when the game is not currently processing a click or is in auto-spin mode.
* If the conditions are met, the button scale is reduced to 0.9, the `audioPlayButton` method of the scene is called to play a sound effect, and the line count is incremented or reset depending on its current value.
* The `txtCountLine` text is updated to display the new line count, and the bet amount is updated in the `txtCountMaxBet` text object of the `maxBet` instance.

**Pointer Up Event:**

* The `pointerup` event listener sets the button scale back to 1 when the pointer is released.

The `addLine` method ensures that the line bet button and text elements are properly created, positioned, and interactive, allowing the player to adjust the line bet amount during gameplay.
