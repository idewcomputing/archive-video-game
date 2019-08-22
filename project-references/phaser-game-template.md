# Phaser Game Template

This section provides instructions and starter code to create a Phaser game template \(HTML, CSS, and JS files with starter code\).

**NOTE:** Your Phaser game has to run on a web server, which means you need to have your HTML, CSS, and JS files hosted online. There are several options:

* use an online code editor that allows you to upload asset files \(for images & sounds\), such as:  [repl.it](https://repl.it/), etc.
* use a desktop code editor and upload your files to your own web host
* use a desktop code editor and create a local web server on your computer \([MAMP](https://www.mamp.info/en/), etc.\)

1. Create a new folder for your game containing a new HTML file \(**index.html**\), new CSS file \(**style.css**\), and new JS file \(**code.js**\).  Be sure to give your game folder a unique name.
   * If you're using Editey, just create a new Editey blank project in Google Drive, which will create a new folder \(called "Blank"\) containing this three files.
2. Add a copy of Phaser \(**phaser.min.js**\) into the folder. If necessary, [download the latest version of Phaser CE from Phaser.io](http://phaser.io/download/release/2.13.1).
   * If you cannot access Phaser.io, you can [download this archived version of Phaser CE](https://drive.google.com/open?id=188YGtmXX2i_ijT0kEP4_7gM9YizExv4a) \(version 2.13.1 — released May 15, 2019\) from Google Drive. Click the download icon to save the file to your computer \(and then add a copy of the file to your game folder in your code editor\).
3. Inside your game folder, create another folder called **assets** \(for images, sounds, tilemaps, etc.\).
4. * **RECOMMENDED**: Inside the **assets** folder, create subfolders \(with names such as **images** and **sounds**\) to keep your asset files better organized when creating your own games.
   * **EXCEPTION:** For the Practice assignments \(1-5, 1-6, 1-7\), you do not need to create an **assets** folder. Instead, you will download a zip file of a provided **assets** folder, and place the extracted folder \(which will contain image files and sound files\) inside your game folder.
5. Copy-and-paste the starter code provided below into your blank HTML, CSS, and JS files.  This represents the "bare bones" code needed to create a webpage with a blank Phaser game canvas.
   * If your HTML, CSS, or JS files already have code in them, delete the code, and replace it with the starter code provided below.
6. Preview your HTML file in an online web editor to verify that a blank Phaser game canvas appears \(i.e., a solid black box\).  If so, you can start coding your game in the JS file by adding Phaser commands inside the `preload()`, `create()`, and `update()` functions.
   * Be sure to add your game asset files \(i.e., images, sounds, etc.\) into the **assets** folder \(or the appropriate subfolder within this folder\).

## HTML file \(index.html\)

```markup
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>My Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div id="my-game"></div>
    <script src="phaser.min.js"></script>
    <script src="code.js"></script>
</body>
</html>
```

* Phaser will insert your game into `<div id="my-game></div>`. You can change the id name, but be sure that your CSS and the Phaser.Game object in your JS use the same id name as your HTML.
* Be sure that the Phaser `<script>` tag is listed before your game code `<script>` tag, so they will load in that order \(otherwise, your game won't work\).
* If you want, you can add other content \(text, images, etc.\) to the webpage by inserting other HTML elements within the body before or after `<div id="my-game></div>`.

## CSS file \(style.css\)

```css
#my-game {
    /* optional */
    width: 800px;
    height: 600px;
    margin: 0 auto;
    border: 1px solid black;
}
```

* The CSS shown above is **optional**, but this styling will center the game display horizontally within the webpage and add a thin black border around it.
* If your CSS lists a width and height for your game, be sure it matches the width and height used to create the Phaser Game object in your JS.
* If you want, you can add CSS to style other elements on the webpage around the game display \(e.g., set a background color for the body, etc.\).

## JS file \(code.js\)

```javascript
// create Phaser.Game object named "game"
var game = new Phaser.Game(800, 600, Phaser.AUTO, 'my-game',
    { preload: preload, create: create, update: update });

// declare global variables for game


// preload game assets - runs once at start
function preload() {

}

// create game world - runs once after "preload" finished
function create() {

}

// update gameplay - runs in continuous loop after "create" finished
function update() {

}

// add custom functions (for collisions, etc.)
```

* You set the width and height \(in order\) of the game display when you create the Phaser.Game object. In the code above, the game display will be 800 pixels in width by 600 pixels in height. You can change these numbers for your game.
* The size of the game world \(which can extend beyond the game display\) can be set using a separate command \(not shown above\).
* You will need to declare global variables for certain game objects \(e.g., the player, a group of enemies, the score, etc.\). These global variables are traditionally listed at or near the top of your JS code before the `preload()` function.
  * If desired, you can avoid global variables by creating variables inside the game object functions using the `this` keyword.  \(However, if you are a beginning programmer, it will be simpler to just use global variables.\)
* A basic Phaser game has 3 core functions: `preload()`, `create()`, and `update()`. You will add your own Phaser code inside these functions:
  * **PRELOAD:** The `preload()` function is used to load game assets \(such as images, sounds, etc.\) into the game's memory, so they are all ready at the same time to be used. The `preload()` function runs one time at the start of the game \(i.e., when the webpage is loaded or refreshed\).
  * **CREATE:** The `create()` function is used to create your game world and its user interface by adding images and sounds into the game, adding player input controls, adding text, etc. The `create()` function runs one time after the `preload()` function is finished.
  * **UPDATE:** The `update()` function is used to update the gameplay by checking for player input, checking for conditions or events, updating objects in the game, etc.  The `update()` function runs in a continuous loop after the `create()` function is finished. The `update()` function loops many times per second, allowing for smooth animations and responsive interactions.
* Most Phaser games will also have custom functions that you create to do certain things based on conditions or events that occur in the game \(such as the player colliding with an enemy, etc.\). These custom functions are traditionally listed at the bottom of your JS code after the `update()` function.

**GAME STATES:** Phaser allows you to break up your game into different states. For example, you could have separate game states for: loading, menu, level 1, level 2, level 3, game over, etc. This template is for a **single-state game**, which is simpler to code if you are new to creating games.

## Troubleshooting

After preparing your Phaser game template, you should see a blank game canvas \(i.e., solid black box\) when you preview your webpage in an online web editor.

**PHASER NOT LOADING?** If you only see a black outline around a white box \(or see nothing at all\), then Phaser isn't loading correctly. Try these troubleshooting tips:

* Be sure you downloaded the correct Phaser JS file: **phaser.min.js**
* Be sure your Phaser JS file is named: **phaser.min.js** — if necessary, rename the file in your game folder to this exact name.
* Be sure you placed **phaser.min.js** into the same folder that has your HTML, CSS, and JS files. If Phaser is in a different folder \(or inside a subfolder\), it will not load \(unless you modify the `<script>` tag in your HTML file to load Phaser from its correct folder location\).
* Be sure your **index.html** file includes `<script>` tags to load **phaser.min.js** and **code.js** \(in that order\) from your game folder. Also be sure the filename listed in the second HTML `<script>` tag matches the actual name of the JS file containing your game code. For example, instead of **code.js**, you could use a different name for your JS file, such as:  **script.js** or **game.js** \(etc.\)
* Be sure your **code.js** file lists the command to create a Phaser.Game object and includes the `preload()`, `create()`, and `update()` functions \(must be present even if empty at first\)
* Be sure your HTML, CSS, and JS files are running on a web server \(such as: Editey web editor in Google Drive, CodePen, Codeanywhere, repl.it, etc.\).
* Check your browser's JavaScript console to see if there are errors listed.

