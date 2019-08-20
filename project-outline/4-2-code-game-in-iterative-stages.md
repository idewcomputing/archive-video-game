# 4-2 Code Game in Iterative Stages

![](../.gitbook/assets/phaser.png)

Your team's Programming Lead will lead the programming of your video game. You'll need to code the game in iterative stages — by adding, testing, and refining one feature at a time. You'll start with the core game mechanics, and work stepwise to bring your game design to "life" in code.

Your team's Art Lead will need to provide the Programming Lead with a scale map of the game's level and a scale layout of the game's user interface.

The Programming Lead will work with your team's Art Lead to:

* add the visual and audio assets into the game \(such as: animated sprites, sound effects, etc.\)
* verify the layout and appearance of the game level and user interface \(and make any necessary changes\)
* add any necessary visual effects into the game \(such as: parallax, particles, tweens, etc.\)

Your team's Research Lead can assist with testing the game during coding as another "pair of eyes" to check for any bugs or possible improvements.

## Before Coding — Plan Out Your Code

Before coding a complex computer program, developers \(aka coders or programmers\) plan out the code in some way — just as you might create an outline to plan out an essay or article before writing it. The plan or outline for a computer program often consists of **pseudocode** or a **flowchart** that describe how the program should work.

It is best to first focus on outlining your core gameplay — which will become the code in your game's `update()` function \(as well as code in your custom functions for collisions, etc.\).

Keep in mind that a program can usually be coded or structured in different ways — and still work correctly. However, sometimes there are certain steps that have to occur in a specific order to make logical sense — otherwise, the program won't work correctly.

### Pseudocode

**Pseudocode** is an informal step-by-step description of what a computer program should do. Pseudocode is often written in plain language \(i.e., English — or whatever language you normally use\). Some developers use a simplified form of "code-like" language to write their pseudocode. Other developers combine plain language plus "code-like" language in their pseudocode.

**EXAMPLE:** [Pseudocode for Space Invaders gameplay](https://drive.google.com/open?id=0B8MTiM_lFG9TOS1Mc1ZvR3RTVnM)

The advantage of writing pseudocode is it makes it easier for you to figure out how your program will work — _before_ you have to write any actual code. Pseudocode is shorter, simpler to understand, and doesn't require all the details to make sense — whereas actual computer code is longer, more abstract, and requires precise logic and syntax to work correctly.

In fact, many developers include some or all of their pseudocode in their actual code by writing each line of pseudocode as a [comment](https://www.w3schools.com/js/js_comments.asp). For example, in JavaScript, you can make a comment by typing two forward slashes `//` at the beginning of the line. Developers then add the actual code below each comment line. The pseudocode comments help the developer as he or she is creating the code, and it also helps explain the code to anyone that reviews \(or revises\) the code later.

**VIDEO:** [Using Pseudocode to Create Actual Code](https://www.khanacademy.org/computing/computer-programming/programming/good-practices/p/planning-with-pseudo-code)

### Flowchart

A **flowchart** is a diagram representing the steps and decisions in a process, such as a computer program \(or part of a program\). Each step in the process is represented by a shape with a brief text description. Different shapes are used to represent: process steps, input or output of data, decision points, etc. The shapes are connected by arrows to represent the path \(or flow\) of the process. The path can split into different branches \(at decision points\), and different branches can also merge back together.

**EXAMPLE:** [Flowchart of Space Invaders gameplay](https://drive.google.com/open?id=0B8MTiM_lFG9TUXpCYU9ISlpnODA)

A flowchart has all the advantages of pseudocode, plus the flowchart visualizes how your program will work. It may be easier to verify your program's logic \(and catch any mistakes\) using a flowchart.

However, a flowchart may take more time to create. It may be faster and easier to first write pseudocode \(and then turn that into a flowchart\).

You can create a flowchart using web apps such as [Google Drawings](https://docs.google.com/drawings/) \(available within Google Drive\), [Draw.io](https://www.draw.io/), [LucidChart](https://www.lucidchart.com/), etc. Of course, you can also draw a flowchart by hand.

**TUTORIAL:** [What is a Flowchart?](https://www.lucidchart.com/pages/what-is-a-flowchart-tutorial)

## During Coding — Code in Iterative Stages

Code the game in iterative stages by adding, testing, and refining one feature at a time. It will make it easier to troubleshoot your code if something doesn't work as expected.

Remember that your Phaser game code has three main functions named `preload()`, `create()`, and `update()` — which are used for different purposes. You will progressively add Phaser commands inside these different functions. You will also need to create custom functions for collision events, etc.

For example, to add the player sprite to the game, you'll need to:

* declare a global variable for the player, such as: `player`
* load an image or spritesheet for the player in the `preload()` function
* add `player` as a sprite in the `create()` function
* set values for certain properties \(such as: gravity, etc.\) of `player` in the `create()` function

After adding the code for a feature, test it out in your game to see whether you need to revise the code \(and retest it\), so the feature works how you expect.

For example, if the newly-added player sprite isn't located in the correct position in the game world, adjust the sprite's position in the code, and refresh the preview of the game to see your changes. Of course, later, you might need to change some of these properties again — or to add other properties or commands — but get the basics of this feature working before coding the next feature.

**RECOMMENDED:** Save back-up versions of your `code.js` file at least once per day — in case you encounter major issues in your code and want to revert back to an earlier version. For example, make a copy of the file, and rename the copy by adding the date \(MM-DD-YY\) to its filename, such as: `code-10-03-17.js`

**OPTIONAL:** Some people find it helpful to code and test new features in a separate copy of the game code \(sort of like a "sandbox environment"\) before adding the new features into their "official" game code.

#### YOUR TASK

1. Create a **pseudocode outline of the core gameplay**.
   * **OPTIONAL:** Use your pseudocode to create a flowchart of the core gameplay.
2. Obtain the following from your team's Art Lead:
   * **scale map of the game's level** \(game world\) that shows each object, its position, and its size \(if needed, map should include a key to identify the objects\)
   * **scale layout of the user interface** \(within game display\) that shows what information is displayed \(text, icons, etc.\), its position, and its size
3. **Find or create "placeholder" images and sprites** that you'll use temporarily until the Art Lead provides the final visual assets later. \(You can also use placeholder sound effects until you obtain the final audio assets later.\)
   * Ideally, the placeholder images or sprites should be the same size \(width and height\) as the final assets are supposed to be \(or as close to the same size as possible\).
   * Later, once you have the final asset files, the only change you might need to make is to update the filenames of the assets loaded in your `preload()` function.
   * For example, you can temporarily load an existing spritesheet \(from another game\) for the player's character.
   * In fact, all images and sprites are technically rectangles, so you can even just create different colored rectangle images \(of the correct sizes\) to use as your temporary assets.
     * You can use an image editor \(such as [Pixlr Editor](https://pixlr.com/editor/)\) to create rectangles of the correct sizes, and fill each rectangle with a different color \(to represent different characters or objects\).
4. **Code your game in iterative stages** by adding, testing, and refining one feature at a time.
   * Here is a [possible order for coding your game features](https://drive.google.com/open?id=1AWvAWxvkEyZtL9wLChRz5TTpqtw2UwKbwd6_jzb-Z1o).
   * Load your placeholder assets in the `preload()` function.
   * When building your game world in the `create()` function, remember that all visual objects — such as images, sprites, text, etc. — are added to the game display in layers \(meaning they can overlap other objects behind them\). The order in which they are added in the `create()` function determines the stacking of these layers in the game world. Adjust their order as necessary for your game \(e.g., so the player sprite isn't hidden behind a background image, etc.\).
   * Use your pseudocode outline to help create your code for your `update()` function \(and for certain custom functions\).
   * While coding, be sure to continually test your game to identify any bugs or improvements. The Research Lead can also help test the game, so you have multiple reviewers.
   * The Art Lead will help verify the layout and appearance of the game level and user interface — and help make any necessary changes.
   * The Art Lead will also help with adding any necessary visual effects by identifying what each effect should look like — as well as when, where, and how long each effect should occur.
   * When the Art Lead has provided the final visual and audio assets, be sure to revise the game code as needed to use the final asset files.

In a later assignment, your team will have external playtesters evaluate the game.

