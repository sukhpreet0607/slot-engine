## Container Class Documentation

### Table of Contents

* [Container Class](#container-class)
    * [Constructor](#constructor)
    * [randomBetween Method](#randombetween-method)

### Container Class

The `Container` class extends the Phaser `GameObjects.Container` class and represents a single column of symbols in the game. It contains five symbols, each randomly chosen from a set of ten possible symbols.

#### Constructor

The constructor initializes the container with its position, adds it to the scene, and creates the five symbols.

```javascript
constructor(scene, x, y) {
    super(scene, x, y);
    scene.add.existing(this);

    //symbols column
    const symbols1 = scene.add.sprite(0, 0, 'symbols', 'symbols_' + this.randomBetween(0, 9) + '.png');
    const symbols2 = scene.add.sprite(0, - Options.symbolHeight, 'symbols', 'symbols_' + this.randomBetween(0, 9) + '.png');
    const symbols3 = scene.add.sprite(0, - Options.symbolHeight * 2, 'symbols', 'symbols_' + this.randomBetween(0, 9) + '.png');
    const symbols4 = scene.add.sprite(0, - Options.symbolHeight * 3, 'symbols', 'symbols_' + this.randomBetween(0, 9) + '.png');
    const symbols5 = scene.add.sprite(0, - Options.symbolHeight * 4, 'symbols', 'symbols_' + this.randomBetween(0, 9) + '.png');

    this.add([symbols1, symbols2, symbols3, symbols4, symbols5]);
}
```

The constructor:

1. **Calls the parent constructor:** `super(scene, x, y)`
2. **Adds the container to the scene:** `scene.add.existing(this)`
3. **Creates five symbol sprites:**
    * Each sprite is created using `scene.add.sprite()`, using the 'symbols' key for the texture and a randomly generated file name from the set 'symbols_0.png' to 'symbols_9.png'. 
    * Each symbol is positioned vertically, with the first symbol at `(0, 0)` and each subsequent symbol offset by `Options.symbolHeight`.
4. **Adds the symbols to the container:** `this.add([symbols1, symbols2, symbols3, symbols4, symbols5])`

#### randomBetween Method

The `randomBetween` method is a helper method used to generate a random integer between a minimum and maximum value.

```javascript
randomBetween(min, max) {
    return Phaser.Math.Between(min, max); 
}
```

It simply utilizes the `Phaser.Math.Between` function to achieve this functionality.
