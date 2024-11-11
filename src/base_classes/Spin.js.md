## Spin Class Documentation

### Table of Contents

* [Spin Class](#spin-class)
    * [Constructor](#constructor)
    * [clearColor Method](#clearcolor-method)
    * [printResult Method](#printresult-method)
    * [getWinningLines Method](#getwinninglines-method)
    * [getLineArray Method](#getlinearray-method)
    * [mathMoney Method](#mathmoney-method)
    * [resetOptions Method](#resetoptions-method)
    * [symbolValue Method](#symbolvalue-method)
    * [audioPlayWin Method](#audioplaywin-method)
    * [audioPlayLose Method](#audioplaylose-method)
    * [getMoney Method](#getmoney-method)
    * [setTextureWin Method](#settexturewin-method)
    * [setTextWidthWin Method](#settextwidthwin-method)

### Spin Class

The `Spin` class handles the logic for processing the results of a spin in the slot machine game. This includes determining winning lines, calculating winnings, and updating the visual display accordingly.

#### Constructor

The constructor initializes the `Spin` object with a reference to the game scene. It calls the `printResult` and `clearColor` methods to process the spin results and clear any visual effects from the previous spin.

```javascript
constructor(scene) {
    this.scene = scene;
    this.printResult();
    this.clearColor();
}
```

#### clearColor Method

This method clears the tints from various visual elements on the screen, including the base spin background, the auto spin button, the max bet button, the coin button, the line button, and the music and sound buttons.

```javascript
clearColor() {
    this.scene.baseSpin.bgSpin.clearTint();
    this.scene.autoSpin.buttonAuto.clearTint();
    this.scene.maxBet.maxBet.clearTint();
    this.scene.coin.coin.clearTint();
    this.scene.btnLine.btnLine.clearTint();
    this.scene.btnMusic.clearTint();
    this.scene.btnSound.clearTint();
}
```

#### printResult Method

This method retrieves the symbols from the result of the spin and stores them in the `Options.result` array. The symbols are retrieved from the `targets` array of the `columnTween` objects, which are part of either the `autoSpin.tweens` or `baseSpin.tweens` objects. The `Options.result` array stores the symbol names for each reel in a nested array format. After storing the result, it calls the `getWinningLines` method to determine any winning lines.

```javascript
printResult() {
    let s1, s2, s3, s4, s5, autoSpin = this.scene.autoSpin.tweens,
    baseSpin = this.scene.baseSpin.tweens;
    if(autoSpin) {
        s1 = autoSpin.columnTween1.targets[0];
        s2 = autoSpin.columnTween2.targets[0];
        s3 = autoSpin.columnTween3.targets[0];
        s4 = autoSpin.columnTween4.targets[0];
        s5 = autoSpin.columnTween5.targets[0];   
    } else {
        s1 = baseSpin.columnTween1.targets[0];
        s2 = baseSpin.columnTween2.targets[0];
        s3 = baseSpin.columnTween3.targets[0];
        s4 = baseSpin.columnTween4.targets[0];
        s5 = baseSpin.columnTween5.targets[0];
    }
    //push symbols name
    Options.result.push([s1.list[3].frame.name, s1.list[2].frame.name,
    s1.list[1].frame.name],[s2.list[3].frame.name, s2.list[2].frame.name,
    s2.list[1].frame.name],[s3.list[3].frame.name, s3.list[2].frame.name,
    s3.list[1].frame.name],[s4.list[3].frame.name, s4.list[2].frame.name,
    s4.list[1].frame.name],[s5.list[3].frame.name, s5.list[2].frame.name,
    s5.list[1].frame.name]);
    //function winning lines
    this.getWinningLines();
}
```

#### getWinningLines Method

This method iterates through each payline defined in the `Options.payLines` array and checks for winning combinations. It uses a nested loop to traverse the coordinates of each payline and compares the symbols at those coordinates to determine if they form a winning streak. If a streak of three or more matching symbols is found, the payline index is added to the `Options.winningLines` array. The method then plays a winning sound effect, calculates the winnings based on the symbol and streak length using the `mathMoney` method, and displays the winning lines using the `getLineArray` method.

```javascript
getWinningLines() {
    for(let lineIndx = 0; lineIndx < Options.line; 
        lineIndx ++) {
        let streak = 0;
        let currentkind = null;
        for(let coordIndx = 0; coordIndx < Options.payLines[lineIndx].
            length; coordIndx ++) {
            let coords = Options.payLines[lineIndx][coordIndx];
            let symbolAtCoords = Options.result[coords[0]][coords[1]];
            if(coordIndx === 0) {
                currentkind = symbolAtCoords;
                streak = 1;
            } else {
                if(symbolAtCoords != currentkind) {
                    break;
                }
                streak ++;
            }
        }
        //check streak >= 3
        if(streak >= 3) {
            lineIndx ++;
            Options.winningLines.push(lineIndx);
            //audio win
            this.audioPlayWin();
            //function math money
            this.mathMoney(currentkind, streak);
        }
        //audio lose
        this.audioPlayLose();
    }
    //get line array
    this.getLineArray(Options.winningLines);
    //reset Options
    this.resetOptions();
}
```

#### getLineArray Method

This method creates and adds `Sprite` objects representing the winning lines to the `Options.lineArray` array. It iterates through the `lineArr` (array of winning line indices) and creates a `Sprite` object with the appropriate payline image for each index.

```javascript
getLineArray(lineArr) {
    if(!lineArr.length) {
        return;
    }
    for(let i = 0; i < lineArr.length; i++) {
        let lineName = 'payline_' + lineArr[i] + '.png';
        Options.lineArray.push(new Sprite(this.scene, Config.width / 2, 
            Config.height / 2, 'line', lineName));
    }
}
```

#### mathMoney Method

This method calculates the winnings based on the winning symbol and the length of the winning streak. It uses the `symbolValue` method to retrieve the payout value for the symbol and streak length, and then multiplies it by the total bet amount to calculate the total winnings.

```javascript
mathMoney(symbolName, streak) {
    let index = streak - 3;
    if(streak === 3)
        this.symbolValue(symbolName, index); 
    else if(streak === 4) 
        this.symbolValue(symbolName, index);
    else 
        this.symbolValue(symbolName, index);
}
```

#### resetOptions Method

This method resets the `Options` object to its initial state, clearing the winnings, results, and winning lines.

```javascript
resetOptions() {
    //reset win && result 
    Options.win = 0;
    Options.moneyWin = 0;
    Options.result = [];
    Options.winningLines = [];
}
```

#### symbolValue Method

This method retrieves the payout value for a specific symbol based on the streak length. It uses a `switch` statement to match the symbol name to the corresponding payout value in the `Options.payvalues` array.

```javascript
symbolValue(symbolName, index) {
    switch(symbolName) {
        case 'symbols_0.png':
            this.getMoney(Options.payvalues[0][index]);
            break;
        case 'symbols_1.png':
            this.getMoney(Options.payvalues[1][index]);
            break;
        case 'symbols_2.png':
            this.getMoney(Options.payvalues[2][index]);
            break;
        case 'symbols_3.png':
            this.getMoney(Options.payvalues[3][index]);
            break;
        case 'symbols_4.png':
            this.getMoney(Options.payvalues[4][index]);
            break;
        case 'symbols_5.png':
            this.getMoney(Options.payvalues[5][index]);
            break;
        case 'symbols_6.png':
            this.getMoney(Options.payvalues[6][index]);
            break;
        case 'symbols_7.png':
            this.getMoney(Options.payvalues[7][index]);
            break;
        case 'symbols_8.png':
            this.getMoney(Options.payvalues[8][index]);
            break;
        default:
            this.getMoney(Options.payvalues[9][index]);
            break;
    } 
}
```

#### audioPlayWin Method

This method plays the winning sound effect if the music is enabled.

```javascript
audioPlayWin() {
    if (this.scene.audioMusicName === 'btn_music.png') {
        //play audio win
        this.scene.audioObject.audioWin.play();
    }
}
```

#### audioPlayLose Method

This method plays the losing sound effect if the music is enabled.

```javascript
audioPlayLose() {
    if (this.scene.audioMusicName === 'btn_music.png') {
        //play audio lose
        this.scene.audioObject.audioLose.play();
    }
}
```

#### getMoney Method

This method calculates the total winnings based on the payout value and the total bet amount. It updates the `Options.win` variable with the calculated winnings and calls the `setTextureWin` method to display the updated winnings on the screen.

```javascript
getMoney(money) {
    let maxBet = Options.line * Options.coin;
    let payValue = money / Options.line;
    Options.win += (payValue * maxBet);
    this.setTextureWin(Options.win);
}
```

#### setTextureWin Method

This method updates the display of the winnings on the screen. It sets the `Options.moneyWin` variable to the current total winnings, updates the `scene.valueMoney` variable, calculates the width of the text based on the winnings, and creates or updates the `scene.txtWin` text object to display the winnings. It also saves the updated winnings to local storage using the `scene.baseSpin.saveLocalStorage` method.

```javascript
setTextureWin(value) {
    Options.moneyWin = value;
    this.scene.valueMoney += Options.moneyWin;
    //function set width text win
    let width = this.setTextWidthWin();
    //check empty text win
    if (!this.scene.txtWin) {
        this.scene.txtWin = this.scene.add.text(width, Config.height - 130, 'WIN: ' + Options.moneyWin + ' $ ', {
            fontSize : '20px',
            color : '#25a028',
            fontFamily : 'PT Serif'
        });
    } else {
        this.scene.txtWin.destroy();
        this.scene.txtWin = this.scene.add.text(width, Config.height - 130, 'WIN: ' + Options.moneyWin + ' $ ', {
            fontSize : '20px',
            color : '#25a028',
            fontFamily : 'PT Serif'
        });
    }
    //save localStorage
    this.scene.baseSpin.saveLocalStorage();
}
```

#### setTextWidthWin Method

This method calculates the width of the text displaying the winnings based on the value of the winnings. This ensures that the text is properly aligned on the screen, regardless of the amount of the winnings.

```javascript
setTextWidthWin() {
    let width;
    if(Options.moneyWin >= 100000) 
        width = Config.width - 340;
    else if(Options.moneyWin >= 10000) 
        width = Config.width - 335;
    else if(Options.moneyWin >= 1000) 
        width = Config.width - 330;
    else if(Options.moneyWin >= 100) 
        width = Config.width - 322;
    else 
        width = Config.width - 340;
    return width;
}
```