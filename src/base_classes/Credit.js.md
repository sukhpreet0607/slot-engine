## Credit Class Documentation

### Table of Contents

* [Credit Class Overview](#credit-class-overview)
* [Constructor](#constructor)
* [addCredit Function](#addcredit-function)
* [deleteCredit Function](#deletecredit-function)

### Credit Class Overview

The `Credit` class is responsible for handling the display and interaction of the game's credit screen. It uses Phaser's Sprite class to create the necessary visual elements. 

### Constructor

The constructor initializes the `Credit` object with a reference to the scene it belongs to. It then calls the `addCredit` function to create the visual elements.

```javascript
constructor(scene) {
    this.scene = scene;
    this.addCredit();
}
```

### addCredit Function

The `addCredit` function is responsible for creating the visual elements of the credit screen:

1. **Credits Button:** It creates a `Sprite` object representing the "Credits" button, positioned at the bottom right of the screen.
    * The sprite is initialized with the appropriate image and scaling.
    * An event listener is attached to the button that triggers the `audioPlayButton` function in the scene.
2. **Paylines Display:** When the "Credits" button is clicked, it creates a `Sprite` object representing the paylines display, centered on the screen.
    * The sprite is initialized with the appropriate image and set to a higher depth to ensure it is visible on top of other elements.
3. **Exit Button:**  It creates a `Sprite` object representing the "Exit" button, positioned at the bottom right of the screen.
    * The sprite is initialized with the appropriate image, scaling, and depth.
    * An event listener is attached to the button that triggers the `deleteCredit` function when clicked.

```javascript
addCredit() {
    this.credits = new Sprite(this.scene, Config.width - 235, Config.height - 680,
        'about', 'btn-credits.png').setScale(0.7);
    this.credits.on('pointerdown', () => {
        //play audio button
        this.scene.audioPlayButton();
        this.paylines = new Sprite(this.scene, Config.width / 2, Config.height / 2,
            'about', 'palines.png').setDepth(1);
        this.btnExit = new Sprite(this.scene, Config.width - 30, 
                Config.height - 635, 'bgButtons', 'btn_exit.png').
                setScale(0.9).setDepth(1);
        this.btnExit.on('pointerdown', this.deleteCredit, this);     
    });
}
```

### deleteCredit Function

The `deleteCredit` function is responsible for destroying the visual elements created by the `addCredit` function:

1. **Play Audio:** It triggers the `audioPlayButton` function in the scene.
2. **Destroy Elements:** It destroys the "Exit" button and the paylines display using their respective `destroy` methods.

```javascript
deleteCredit() {
    //play audio button
    this.scene.audioPlayButton();
    this.btnExit.destroy();
    this.paylines.destroy();
}
``` 
