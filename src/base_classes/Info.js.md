# Info Class Documentation

## Table of Contents

- [Class Info](#class-info)
    - [Constructor](#constructor)
    - [addInfo()](#addinfo)
    - [showPayTable()](#showpaytable)
    - [showTable()](#showtable)
    - [deleteTable()](#deletetable)

## Class Info

The `Info` class is responsible for managing the "Info" button and its associated paytable functionality.

### Constructor

```javascript
constructor(scene) {
    this.scene = scene;
    this.addInfo();
    this.click = false;
}
```

The constructor initializes the `Info` object with a reference to the scene it belongs to. It also calls the `addInfo()` method to create the "Info" button and sets the `click` flag to `false`.

### addInfo()

```javascript
addInfo() {
    this.info = new Sprite(this.scene, Config.width - 1020, Config.height - 50, 'bgButtons', 'btn-info.png');
    //add bitmap text
    const txtInfo = this.scene.add.dynamicBitmapText(Config.width - 1060, Config.height - 70, 'txt_bitmap', Options.txtInfo, 38);
    txtInfo.setDisplayCallback(this.scene.textCallback);
    this.info.on('pointerdown', this.showPayTable, this);
}
```

This method creates the "Info" button using the `Sprite` class, positions it on the screen, and adds a text label using a `dynamicBitmapText` object. The text is fetched from the `Options.txtInfo` constant and formatted using the `textCallback` function defined in the scene.  The `pointerdown` event listener is attached to the button, calling the `showPayTable()` method when the button is clicked.

### showPayTable()

```javascript
showPayTable() {
    if (!this.click) {
        //set click = true
        this.click = true;
        //play audio button
        this.scene.audioPlayButton();
        //function show table
        this.showTable();
        this.btnExit = new Sprite(this.scene, Config.width - 30,
            Config.height - 635, 'bgButtons', 'btn_exit.png').
            setScale(0.9).setDepth(1);
        this.btnExit.on('pointerdown', this.deleteTable, this);
    }
}
```

The `showPayTable()` method is called when the "Info" button is clicked. It checks if the `click` flag is false (indicating the paytable is not already shown). If it is, the `click` flag is set to `true`, an audio button is played, and the `showTable()` method is called to display the paytable.  An "Exit" button is also created and positioned on the screen, with its `pointerdown` event listener set to call the `deleteTable()` method.

### showTable()

```javascript
showTable() {
    this.payValues = [];

    this.paytable = new Sprite(this.scene, Config.width / 2, Config.height / 2,
        'about', 'paytable.png').setDepth(1);

    var width = 190, width2 = width, height = 25, height2 = 245;

    for (let i = 0; i < Options.payvalues.length; i++) {
        if (i >= 5) {
            for (let j = 0; j < Options.payvalues[i].length; j++) {
                height2 -= 30;
                this.payValues.push(this.scene.add.text(width2, Config.height / 2 + height2, Options.payvalues[i][j], {
                    fontSize: '30px',
                    color: '#630066',
                    fontFamily: 'PT Serif'
                }).setDepth(1));
            }
            width2 += 225;
            height2 = 245;
        } else {
            for (let j = 0; j < Options.payvalues[i].length; j++) {
                height += 30;
                this.payValues.push(this.scene.add.text(width, Config.height / 2 - height, Options.payvalues[i][j], {
                    fontSize: '30px',
                    color: '#630066',
                    fontFamily: 'PT Serif'
                }).setDepth(1));
            }
            width += 225;
            height = 25;
        }
    }
}
```

The `showTable()` method displays the paytable. It first creates an empty array `payValues` to store the paytable text objects. It then creates a `Sprite` object for the paytable image and positions it in the center of the screen.  The code then iterates through the `Options.payvalues` array, which contains the paytable data.  For each entry in the array, it creates a text object using `scene.add.text` and adds it to the `payValues` array. The position of each text object is calculated based on its index in the `Options.payvalues` array, ensuring that the paytable is displayed in a readable format.

### deleteTable()

```javascript
deleteTable() {
    //set click = false
    this.click = false;
    //play audio button
    this.scene.audioPlayButton();
    this.paytable.destroy();
    this.btnExit.destroy();
    if (this.payValues.length > 0) {
        for (let i = 0; i < this.payValues.length; i++) {
            this.payValues[i].destroy();
        }
    }
}
```

The `deleteTable()` method is called when the "Exit" button is clicked. It sets the `click` flag to `false`, plays an audio button, and then destroys the paytable image, the "Exit" button, and all the paytable text objects in the `payValues` array. This effectively hides the paytable from the screen.
