# AutoSpin Class Documentation

## Table of Contents

* [AutoSpin](#autospin)
    * [Constructor](#constructor)
    * [autoSpin](#autospppin)
    * [playSpeedAuto](#playspeedauto)
    * [plus](#plus)
    * [minus](#minus)
    * [play](#play)
    * [exit](#exit)
    * [speedPlay](#speedplay)
    * [setTextAuto](#setautotext)
    * [setXAuto](#setxauto)
    * [removeImgAuto](#removeimgautp)

## AutoSpin

The `AutoSpin` class manages the automatic spinning feature in the game. It provides functionality for starting and stopping automatic spins, adjusting the bet amount, and executing the spinning process at the specified speed.

### Constructor

```typescript
constructor(scene) {
    this.scene = scene;
    this.autoSpin();
}
```

The constructor initializes the `AutoSpin` object with a reference to the game scene. It calls the `autoSpin` method to set up the initial UI elements and event listeners for the auto spin feature.

### autoSpin

```typescript
autoSpin() {
    this.buttonAuto = new Sprite(this.scene, Config.width - 110, Config.height - 50, 'bgButtons', 'btn-info.png');
    this.txtAutoSpin = this.scene.add.dynamicBitmapText(Config.width - 155, Config.height - 70, 'txt_bitmap', Options.txtAutoSpin, 38);
    this.txtAutoSpin.setDisplayCallback(this.scene.textCallback);
    this.buttonAuto.on('pointerdown', () => {
        if (!Options.checkClick) {
            this.buttonAuto.setScale(0.9);
            this.playSpeedAuto();
        }
    });
    this.buttonAuto.on('pointerup', () => this.buttonAuto.setScale(1));
}
```

The `autoSpin` method creates the initial UI elements for the auto spin feature:

*   **`this.buttonAuto`:** A sprite representing the "Auto Spin" button, positioned at the bottom right of the screen.
*   **`this.txtAutoSpin`:** A dynamic bitmap text displaying the current auto spin status ("AUTO" or "STOP").

Event listeners are attached to the "Auto Spin" button:

*   **`pointerdown`:** When the button is pressed down, it scales down to 0.9 and calls the `playSpeedAuto` method.
*   **`pointerup`:** When the button is released, it scales back to 1.

### playSpeedAuto

```typescript
playSpeedAuto() {
    if(Options.txtAutoSpin === 'STOP') {
        //set text auto
        Options.txtAutoSpin = 'AUTO';
        this.txtAutoSpin.setText(Options.txtAutoSpin);
        //remove timer event
        if(this.txtSpeed && this.timer) {
            this.txtSpeed.destroy();
            this.timer.remove();
        }   
    } else {
        //set text auto
        Options.txtAutoSpin = 'STOP';
        this.txtAutoSpin.setText(Options.txtAutoSpin);
        //play audio button
        this.scene.audioPlayButton();

        this.bgAuto = new Sprite(this.scene, Config.width / 2, Config.height / 2,
            'autoSpin', 'bg_auto.png');
        this.auto = new Sprite(this.scene, Config.width / 2, Config.height / 2 - 100,
            'bgButtons', 'btn-spin.png');

        this.txtAuto = this.scene.add.text(Config.width / 2 - 5, Config.height / 2 - 115,
            Options.txtAuto, { fontSize : '35px', color : '#fff', fontFamily : 'PT Serif' });

        this.setXAuto();
        this.plus();
        this.minus();
        this.play();
        this.exit();
    }
}
```

The `playSpeedAuto` method handles the logic for starting and stopping the auto spin:

1.  **Toggle Auto Spin Status:** It checks the current auto spin status (`Options.txtAutoSpin`) and toggles it between "AUTO" and "STOP".
2.  **Remove Previous Timer:** If the auto spin is being stopped, it removes the existing speed timer (`this.timer`) and destroys the speed display text (`this.txtSpeed`).
3.  **Start Auto Spin UI:** If the auto spin is being started, it creates the following UI elements:
    *   **`this.bgAuto`:** A background image for the auto spin UI.
    *   **`this.auto`:** A button for triggering the spin.
    *   **`this.txtAuto`:** A text display for the current bet amount.
4.  **Setup UI Functions:** It calls other methods to handle the UI elements:
    *   **`setXAuto`:** Sets the correct X position for the bet amount text based on its value.
    *   **`plus`:** Creates a button for increasing the bet amount.
    *   **`minus`:** Creates a button for decreasing the bet amount.
    *   **`play`:** Creates a button for initiating a spin.
    *   **`exit`:** Creates a button for exiting the auto spin UI.

### plus

```typescript
plus() {
    this.btnPlus = new Sprite(this.scene, Config.width / 2 - 100, Config.height / 2 - 100,
        'autoSpin', 'btn_plus_bet.png');
    this.btnPlus.on('pointerdown', () => {
        //play audio button
        this.scene.audioPlayButton();
        if(Options.txtAuto < 100) {
            this.btnMinus.clearTint();
            this.btnPlus.setScale(0.9);
            Options.txtAuto += 5;
            //set text x auto
            Options.txtAuto < 100 ? this.txtAuto.x = 620 :
                this.txtAuto.x = 610;
            this.txtAuto.setText(Options.txtAuto);
        }
        if(Options.txtAuto === 100) {
            this.btnPlus.setTint(0xa09d9d);
        }
    });
    this.btnPlus.on('pointerup', () => this.btnPlus.setScale(1));
}
```

The `plus` method creates a button for increasing the bet amount:

1.  **Create Button:** It creates a sprite representing the "+" button, positioned at the left of the bet amount text.
2.  **Event Listeners:** It attaches event listeners to the button:
    *   **`pointerdown`:** When the button is pressed down, it scales down to 0.9, plays an audio effect, increases the bet amount (`Options.txtAuto`) by 5, updates the bet amount text, and adjusts its position. If the bet amount reaches 100, it tints the button gray.
    *   **`pointerup`:** When the button is released, it scales back to 1.

### minus

```typescript
minus() {
    this.btnMinus = new Sprite(this.scene, Config.width / 2 + 100, Config.height / 2 - 100,
        'autoSpin', 'btn_minus_bet.png');
    this.btnMinus.on('pointerdown', () => {
        //play audio button
        this.scene.audioPlayButton(); 
        if(Options.txtAuto > 5) {
            this.btnPlus.clearTint();
            this.btnMinus.setScale(0.9);
            Options.txtAuto -= 5;
            //function set text x auto
            this.setXAuto();
            this.txtAuto.setText(Options.txtAuto);  
        }
        if(Options.txtAuto === 5) {
            this.btnMinus.setTint(0xa09d9d);
        }  
    });
    this.btnMinus.on('pointerup', () => this.btnMinus.setScale(1));
}
```

The `minus` method creates a button for decreasing the bet amount:

1.  **Create Button:** It creates a sprite representing the "-" button, positioned at the right of the bet amount text.
2.  **Event Listeners:** It attaches event listeners to the button:
    *   **`pointerdown`:** When the button is pressed down, it scales down to 0.9, plays an audio effect, decreases the bet amount (`Options.txtAuto`) by 5, updates the bet amount text, adjusts its position, and calls the `setXAuto` method to reposition the bet amount text. If the bet amount reaches 5, it tints the button gray.
    *   **`pointerup`:** When the button is released, it scales back to 1.

### play

```typescript
play() {
    this.btnPlay = new Sprite(this.scene, Config.width / 2, Config.height / 2 + 100,
        'bgButtons', 'btn_play.png').setScale(0.9);
    this.btnPlay.on('pointerdown', () => {
        //play audio button
        this.scene.audioPlayButton();
        //function remove image auto
        this.removeImgAuto();
        if(this.scene.valueMoney >= Options.coin * Options.line) 
            this.speedPlay(Options.txtAuto);
        else
            this.setTextAuto();
    });
}
```

The `play` method creates a button for initiating a spin:

1.  **Create Button:** It creates a sprite representing the "Play" button, positioned below the bet amount text.
2.  **Event Listeners:** It attaches an event listener to the button:
    *   **`pointerdown`:** When the button is pressed down, it plays an audio effect, removes the auto spin UI elements by calling the `removeImgAuto` method, and checks if the player has enough money to place the bet. If enough money is available, it calls the `speedPlay` method to start the spinning process with the specified speed. If not enough money is available, it sets the auto spin status back to "AUTO" by calling the `setTextAuto` method.

### exit

```typescript
exit() {
    this.btnExit = new Sprite(this.scene, Config.width - 30 , 
        Config.height - 635,
        'bgButtons', 'btn_exit.png').setScale(0.9);
    this.btnExit.on('pointerdown', () => {
        //play audio button
        this.scene.audioPlayButton();
        //function remove image auto
        this.removeImgAuto();
        //set text auto
        this.setTextAuto();
    });
}
```

The `exit` method creates a button for exiting the auto spin UI:

1.  **Create Button:** It creates a sprite representing the "Exit" button, positioned at the bottom right of the screen.
2.  **Event Listeners:** It attaches an event listener to the button:
    *   **`pointerdown`:** When the button is pressed down, it plays an audio effect, removes the auto spin UI elements by calling the `removeImgAuto` method, and sets the auto spin status back to "AUTO" by calling the `setTextAuto` method.

### speedPlay

```typescript
speedPlay(speed) {
    //set text speed
    let width;
    speed > 5 ? width = Config.width - 150 :  width = Config.width - 130;

    this.txtSpeed = this.scene.add.dynamicBitmapText(width, Config.height / 2 - 350, 'txt_bitmap', speed, 80);
    this.txtSpeed.setDisplayCallback(this.scene.textCallback);
    this.timer = this.scene.time.addEvent({
        delay: 500,
        callback: function() {
            //set delay 
            this.timer.delay = 4500;
            if(speed > 0 && this.scene.valueMoney >= 
                Options.coin * Options.line) {
                //set color
                this.scene.baseSpin.setColor();
                //set check click = true
                Options.checkClick = true;
                //detroys line array
                this.scene.baseSpin.destroyLineArr();
                //funtion remove text win
                this.scene.baseSpin.removeTextWin();
                //save localStorage
                this.scene.baseSpin.saveLocalStorage();
                this.tweens = new Tween(this.scene);
                speed -- ;
                this.txtSpeed.setText(speed);
            } else {
                Options.checkClick = false;
                this.timer.remove(false);
                this.txtSpeed.destroy();
                //set text auto
                this.setTextAuto();
            }
        },
        callbackScope: this,
        loop: true
    });
}
```

The `speedPlay` method handles the automatic spinning process at the specified speed:

1.  **Create Speed Display:** It creates a dynamic bitmap text (`this.txtSpeed`) to display the current speed, positioned at the top of the screen.
2.  **Start Timer:** It creates a timer (`this.timer`) that triggers a callback function every 500 milliseconds.
3.  **Timer Callback:** The callback function handles the following:
    *   **Set Delay:** Sets the timer delay to 4500 milliseconds (4.5 seconds) after the first execution.
    *   **Check Conditions:** It checks if the speed is greater than 0 and the player has enough money to bet.
    *   **Perform Spin:** If both conditions are met, it executes the following:
        *   **Set Color:** Calls a method to set the colors for the spinning symbols.
        *   **Enable Click:** Sets `Options.checkClick` to `true` to enable click interactions.
        *   **Destroy Lines:** Destroys any existing win lines.
        *   **Remove Win Text:** Removes any existing win text.
        *   **Save Local Storage:** Saves the game data to local storage.
        *   **Create Tween:** Creates a new tween object to handle animation.
        *   **Decrement Speed:** Decreases the speed by 1 and updates the speed display.
    *   **Stop Spin:** If either condition is not met, it stops the auto spin:
        *   **Disable Click:** Sets `Options.checkClick` to `false` to disable click interactions.
        *   **Remove Timer:** Removes the timer.
        *   **Destroy Speed Text:** Destroys the speed display text.
        *   **Set Auto Text:** Sets the auto spin status back to "AUTO" by calling the `setTextAuto` method.

### setTextAuto

```typescript
setTextAuto() {
    Options.txtAutoSpin = 'AUTO';
    this.txtAutoSpin.setText(Options.txtAutoSpin);
}
```

The `setTextAuto` method sets the auto spin status text back to "AUTO". It updates the `Options.txtAutoSpin` variable and sets the text of the auto spin status display (`this.txtAutoSpin`) to "AUTO".

### setXAuto

```typescript
setXAuto() {
    if(Options.txtAuto >= 100) 
        this.txtAuto.x = 610;
    else if(Options.txtAuto >= 10)
        this.txtAuto.x = 620;
    else 
        this.txtAuto.x = 635;
}
```

The `setXAuto` method adjusts the X position of the bet amount text (`this.txtAuto`) based on its value:

*   **100 or more:** Sets the X position to 610.
*   **10 to 99:** Sets the X position to 620.
*   **Less than 10:** Sets the X position to 635.

### removeImgAuto

```typescript
removeImgAuto() {
    this.bgAuto.destroy();
    this.btnPlus.destroy();
    this.btnMinus.destroy();
    this.auto.destroy();
    this.txtAuto.destroy();
    this.btnPlay.destroy();
    this.btnExit.destroy();
}
```

The `removeImgAuto` method destroys all the UI elements related to the auto spin feature:

*   **`this.bgAuto`:** The background image.
*   **`this.btnPlus`:** The "+" button.
*   **`this.btnMinus`:** The "-" button.
*   **`this.auto`:** The spin button.
*   **`this.txtAuto`:** The bet amount text.
*   **`this.btnPlay`:** The "Play" button.
*   **`this.btnExit`:** The "Exit" button.