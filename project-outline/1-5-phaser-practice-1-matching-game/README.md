# 1-5 Phaser Practice 1: Matching Game

It's time to introduce you to the technology that your team will be using later to develop it's own game. This will be your first practice coding a browser-based video game using the [Phaser JS game engine](http://phaser.io/).

For this practice, all the game assets \(i.e., images and sounds\) and nearly all the game code will be provided. Focus on becoming familiar with how the code works.

This first practice will create a simple game. You're going to code an emoji matching game, similar to a slot machine. This game is based entirely on chance — no player skill involved. However, it will introduce you to some of the basic concepts of using Phaser to create a game.

![](../../.gitbook/assets/emoji-match-logo.png)

![](../../.gitbook/assets/emoji-match.png)

**PREVIEW VIDEO:** [Demo of Emoji Match](https://drive.google.com/open?id=0B8MTiM_lFG9TOEZ3XzNOU3N6anc)

## PREP STEPS

1. Check out the [Phaser Introduction](../../project-references/phaser-introduction.md) to better understand what Phaser does and to download a copy of the latest version of Phaser \(**phaser.min.js**\).
2. Follow the instructions to prepare your [Phaser Game Template](../../project-references/phaser-game-template.md).
3. Download this [assets.zip](https://drive.google.com/open?id=0B8MTiM_lFG9TTTFWZktKa0U4VEU) file, and then extract \(decompress\) the file contents, which will be a folder named **assets** that contains one image file and three sound files. \(To extract the file on a Windows computer, right-click on the downloaded zip file, and select _Extract All_. On a Mac, double-click on the downloaded zip file.\)
4. Place the **assets** folder into your game template folder.
5. Test your Phaser game template by previewing the HTML file online. If everything's ready to go, you should see a **solid black box** \(i.e., a blank Phaser game canvas\) on your webpage.

**PHASER NOT LOADING?** If you only see a black outline around a white box \(or see nothing at all\), then Phaser isn't loading correctly. Try these troubleshooting tips:

* Be sure you downloaded the correct Phaser JS file: **phaser.min.js**
* Be sure your Phaser JS file is named: **phaser.min.js** — if necessary, rename the file in your game folder to this exact name.
* Be sure you placed **phaser.min.js** into the same folder that has your HTML, CSS, and JS files. If Phaser is in a different folder \(or inside a subfolder\), it will not load \(unless you modify the `<script>` tag in your HTML file to load Phaser from its correct folder location\).
* Be sure your **index.html** file includes `<script>` tags to load **phaser.min.js** and **code.js** \(in that order\) from your game folder.
* Be sure your **code.js** file lists the command to create a Phaser.Game object and includes the `preload()`, `create()`, and `update()` functions \(must be present even if empty at first\)
* Be sure your HTML, CSS, and JS files are running on a web server \(such as: Editey web editor in Google Drive, CodePen, Codeanywhere, etc.\).
* Check your browser's JavaScript console to see if there are errors listed.

## CODING STEPS

This first practice game will be coded in 10 steps. Overall, it doesn't take that much code to create this game; however, every step will explain how the code works.

In Step 1, you'll make a couple of changes to your HTML file and CSS file to demonstrate that the rest of the webpage surrounding your game can be modified. That will be useful later when your team develops its own game — you'll be able to customize the webpage to match your game's theme.

The rest of the steps are for coding your game, which will be done in your **code.js** file using Phaser JavaScript commands \(as well as regular JavaScript\).

The steps are outlined below. The instructions for Steps 1-5 start on the next page.

### Step 1: Modify HTML and CSS

### Step 2: Look at Starter Phaser JS Code

### Step 3: Change Background Color of Game

### Step 4: Add Emoji Sprite Using Image

### Step 5: Add Player Input

### Step 6: Add Sound Effect

### Step 7: Add Text to Game

### Step 8: Add Other Emoji Sprites

### Step 9: Add Custom Function to Check for Matches

### Step 10: Add Improvements to Game

