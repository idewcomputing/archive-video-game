# Phaser Introduction

## What is Phaser?

![](../.gitbook/assets/phaser.png)

[Phaser](http://phaser.io/) is a JavaScript library that makes it easier to code a game — similar to how [jQuery](https://jquery.com/) is a JavaScript library that makes it easier to modify HTML and CSS on a webpage.

Phaser makes it easy to add your game's graphics to build a game world, add animations, add player inputs, add physics to your game world \(velocity, gravity, friction, etc.\), detect collisions between game objects, add particle explosions, play sounds, and so much more.

Phaser creates 2D games. However, if you're new to creating games, it's better to start with 2D.

## How does Phaser work?

All you need to do is load the Phaser JS file into your webpage using a `<script>` tag, and then start using Phaser commands in your game code JS file.

The Phaser JS file contains code defining different types of JavaScript objects. An **object** is a special type of variable that can contain a set of **properties** \(variables\) and **methods** \(functions\).

In fact, a property \(a variable\) within an object can be another object — in other words, an object can contain other objects inside it.

Phaser objects have been specifically designed to have properties and methods that are useful for coding video games.

Every Phaser command is a reference to a Phaser object property or a Phaser object method. If the command has parentheses, then it is a reference to a method. Otherwise, it is a reference to a property.

**RESOURCE:** [W3Schools has a great explanation of JavaScript Objects](https://www.w3schools.com/js/js_objects.asp)

Phaser creates your game by inserting an HTML `<canvas>` element into your webpage. The `<canvas>` element is a container that allows you to add graphics and text inside it using JavaScript. Things added inside the `<canvas>` element can be animated and interactive, which makes it perfect to use for a video game.

**RESOURCE:** [If you're interested in learning more, W3Schools has an HTML Canvas tutorial](https://www.w3schools.com/graphics/canvas_intro.asp)

## Where can I download Phaser?

You will need a copy of Phaser CE \(**phaser.min.js**\) inside your game folder, which will also contain your HTML, CSS, and JS files.

[Get the latest version of Phaser CE from Phaser.io](http://phaser.io/download/release/2.13.1). Click the "min.js" icon to download the file.

If you cannot download from Phaser.io, you can [download this archived version of Phaser CE](https://drive.google.com/open?id=188YGtmXX2i_ijT0kEP4_7gM9YizExv4a) \(version 2.13.1 — released May 15, 2019\) from Google Drive. Click the download icon to save the file to your computer \(and then add a copy of the file to your game folder in your code editor\).

{% hint style="danger" %}
**PHASER 3 VS. PHASER CE:**  Phaser 3 is the newest version of Phaser \(first released in February 2018\), which is maintained and updated by the folks at Photon Storm. Phaser CE is an open-source version of the previous Phaser 2, which is maintained and updated by the Phaser developer community.

While the API methods for Phaser 3 are similar to Phaser CE, code written for one will **not** work in the other without modification. All of the code examples in this project guidebook are based on **Phaser CE**. Eventually, this guidebook will be revised to use Phaser 3, which does offer improvements. However, in the meantime, be sure to use Phaser CE for this project.
{% endhint %}

## Where can I learn how to use Phaser?

Besides the Phaser coding references available in this GitBook, here are additional online resources for learning more about using Phaser CE:

* [Phaser CE API Documentation on GitHub](https://photonstorm.github.io/phaser-ce/)
* [Phaser CE Code Examples on Phaser.io](http://phaser.io/examples) \(Phaser 2 = Phaser CE\)
* [Phaser CE Code Examples on CodePen](https://codepen.io/collection/AMbZgY/)
* [Phaser community tutorials on Phaser.io](http://phaser.io/news/category/tutorial) \(older examples prior to 2018 use Phaser 2, but newer examples use Phaser 3\)
* [Phaser Game Examples by Emanuele Feronato](https://www.emanueleferonato.com/category/phaser/) \(her older examples prior to 2018 use Phaser CE, but newer examples use Phaser 3\)

{% hint style="info" %}
**PHASER CE / PHASER 2:**  You can also find other Phaser resources online. Just keep in mind that Phaser 3 was released in February 2018, so newer resources might use Phaser 3 \(which is **not** compatible with Phaser CE\). Phaser resources published prior to 2018 will use Phaser 2 \(which is now referred to as Phaser CE\).
{% endhint %}

## Why use Phaser instead of another game engine?

There are [other JS game engines](http://html5gameengine.com/) available for creating browser-based games. There are also numerous [game engines](https://en.wikipedia.org/wiki/List_of_game_engines) \(such as: Unity, etc.\) for making games for other platforms \(consoles, PC, iOS, Android, etc.\). So why use Phaser?

Some **advantages** of using Phaser:

* It has lots of features for developing full-fledged games
* It uses JavaScript \(many cross-platform game engines use C++\)
* It's free and open-source
* It's updated frequently to fix bugs and add new features
* Its capabilities can be extended with plugins and integrations with other software
* It has a large community of users \(thus lots of online help available\)
* There are thousands of [games made with Phaser](http://phaser.io/news/category/game) \(see what's possible and get inspired\)
* It's easy enough for beginners but powerful enough for seasoned developers
* It's perfect for learning how to prototype and develop games — and can serve as a springboard for those who are more serious about game design and game development

Some **limitations** of using Phaser:

* It is limited to creating browser-based games \(though you can [convert your web app game into a native mobile app](https://phonegap.com/)\)
* It is limited to creating 2D games \(though you can [create pseudo-3D isometric games](https://phaser.io/news/2017/05/creating-isometric-worlds-tutorial-part-1)\)
* It is limited to single player games "out of the box" \(though you can [create multiplayer games with Phaser](http://www.dynetisgames.com/2017/03/06/how-to-make-a-multiplayer-online-game-with-phaser-socket-io-and-node-js/) if you're more adventurous\)

Overall, Phaser offers many advantages for learners new to creating video games, which is why it is utilized in this project.

However, this project could be modified to use other available game engines. The game design process will be similar, regardless of the specific game engine used for development.

