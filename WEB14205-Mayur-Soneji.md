## Info

**Name:** Mayur Soneji

**Student Number:** 96455016

**Course:** WEB14205


## Blogs

[What is your favourite board game and why?](https://medium.com/@m.soneji98/what-is-your-favourite-board-game-and-why-dae99784b61f) 

[When creating online content for children, what are some of the ethical considerations you need to take?](https://medium.com/@m.soneji98/when-creating-online-content-for-children-what-are-some-of-the-ethical-considerations-you-need-to-9dfd75880378) 

[Strengths And Weaknesses In My Current Project](https://medium.com/@m.soneji98/what-are-your-strength-and-weaknesses-in-relation-to-completing-this-project-d327877e636f)  

[Reflect and evidence your contribution to this group project so far.](https://medium.com/@m.soneji98/reflect-and-evidence-your-contribution-to-this-group-project-so-far-14964f9a1dda)

[Analysis Of How Games Have Developed How Game](https://medium.com/@m.soneji98/evidence-and-analyse-how-games-have-developed-through-time-240f3d5deb3c) 

[500 Word Reflective Report](https://medium.com/@m.soneji98/500-words-reflective-report-on-your-why-what-where-when-how-contribution-to-this-project-d93ee03bb95a) 

[500 words on core game mechanic, experience goals, objective and rules of CS:GO](https://medium.com/@m.soneji98/500-words-dissect-and-discuss-core-game-mechanics-behind-an-online-game-of-their-choice-ca8041465d3a)


## Idea

We decided to take Monopoly and repurpose it with daily life. The aim of the game is to teach users the importance of budgeting as there are unpredictable things that occur in daily life, for example, smashing a phone.


## Handin

- [Research Pack](https://docs.google.com/document/d/1AfkIMZAGdXI5wCPHegwTwPmKCzBGqBp2Il9nGQvZl5k/edit?usp=sharing)

- [Media Assets](https://drive.google.com/drive/folders/1sBYS45D7Q4Wt4emkJBNZxFP4TZeJysp_?usp=sharing)

- [Board Game Production](https://drive.google.com/file/d/19zbT8dtb7lVI-O0zdVluQYlZEkeua-MO/view?usp=sharing)

- [Digital Game Production](https://mayursoneji.github.io/taxevasion/)

- [Testing Report](https://docs.google.com/document/d/1utepHqfXlxd-1PgpJP1yDtzTtrBuzLk4M5lb5pZuaQw/edit?usp=sharing)

- [Formative Presentation](https://docs.google.com/presentation/d/1rYyrymvfC0PvxwtIGSZ5H7Qu7t9-xqXPAUZh30Hb7RY/edit?usp=sharing)

- [Summative presentation](https://docs.google.com/presentation/d/1DheuLLBjkBjEEpQVFe1_kSDe7YJ9UXmQt1zd3s29-2Y/edit?usp=sharing)

## Phaser

The game plays on the board game mentioned above. With the idea of Monopoly, the player becomes bankrupt, the same end goal is in Way Of Life. Since someone would be in debt we wanted to play on the idea of cat and mouse by having a debt collector chasing the player.

This is the initial code used for the Phaser production, it's from Phaser 2 as there was limited documentation for phaser 3. The initial code had the debt collector chasing the person in debt at a steady pace and jumping consistently in terms of time and height. The code was used as a platform for me to learn Phaser while still learning to produce a final game:

~~~~
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <title>A Bobby's Tale - By wwwtf productions</title>
    <script type="text/javascript" src="js/phaser.min.js"></script>
       <style type="text/css">
        body {
            margin: 0;
        }
    </style>
</head>
<body>
 
<script type="text/javascript">
var game = new Phaser.Game(800, 600, Phaser.AUTO, 'phasergame', { preload: preload, create: create, update: update });
   
function preload() {
    game.load.image('city', 'Assets/city1.png');
    game.load.image('ground', 'Assets/platform4.png');
    game.load.image('clue', 'Assets/clue.png');
    game.load.image('floor', 'Assets/platform4.png');
    game.load.image('barrier', 'Assets/barrier.png');
    game.load.image('crate', 'Assets/crate.png');
    game.load.image('pause', 'Assets/pausebutton.jpg');
    game.load.spritesheet('police', 'Assets/policesprite.png', 32, 48);
    game.load.spritesheet('clapp', 'Assets/villian.png', 32, 48);
    game.load.spritesheet('startButton','Assets/startbutton.png');
}
var camSpeed = 2;
   
var button;
var player;
var platforms;
var cursors;
var villain;
var clues;
var score = 0;
var scoreText;
var timer;
var seconds = 0;
var minutes = 0;
var timerText;
function create() {
   
    //  We're going to be using physics, so enable the Arcade Physics system
    game.physics.startSystem(Phaser.Physics.ARCADE);
   
    //setting the world size - not just camera view like before
    game.world.setBounds(0, 0, 4000, 600);
    //  A simple background for our game
    game.add.tileSprite(0, 0, 4000, 600, 'city');
   
    //setting the colour of the 'sky'
    game.stage.backgroundColor = "#9ab4d7";
   
     // start button
    Button = game.add.button(100, 150, 'startButton', actionOnClick, this, 2, 1, 0);
    Button.scale.setTo(0.4,0.4);
    function actionOnClick () {
    Button.visible = false;
    player.visible = true;
    villain.visible = true;
    clue.visible = true;
    ground.visible = true;
    platforms.visible = true;}
   
    //  The platforms group contains the ground and the 2 ledges we can jump on
    platforms = game.add.group();
    //  We will enable physics for any object that is created in this group
    platforms.enableBody = true;
    // Here we create the ground.
    var ground = platforms.create(0, game.world.height - 34, 'floor');
    //  Scale it to fit the width of the game (the original sprite is 400x32 in size)
    ground.scale.setTo(1,1);
    //  This stops it from falling away when you jump on it
    ground.body.immovable = true;
   
    //creating a ledge for the villain to stand on at the start of the game.
    var ledge = platforms.create(-3960, 200, 'ground');
    ledge.body.immovable = true;
   
    //adding traffic barriers for the player to jump over
    var barrier = platforms.create(400, 530, 'barrier');
    barrier.body.immovable = true;
   
    var barrier = platforms.create(1700, 530, 'barrier');
    barrier.body.immovable = true;
   
    //adding crate for player to jump over
    var crate = platforms.create(1100, 550, 'crate');
    crate.body.immovable = true;
   
    var crate = platforms.create(2900, 550, 'crate');
    crate.body.immovable = true;
   
    var crate = platforms.create(3400, 550, 'crate');
    crate.body.immovable = true;
    // The player and its settings
    player = game.add.sprite(32, game.world.height - 150, 'police');
    //  We need to enable physics on the player
    game.physics.arcade.enable(player);
    //  Player physics properties. Give the little guy a slight bounce.
    player.body.bounce.y = 0.2;
    player.body.gravity.y = 300;
    player.body.collideWorldBounds = true;
    //  Our two animations, walking left and right.
    player.animations.add('left', [0, 1, 2, 3], 10, true);
    player.animations.add('right', [5, 6, 7, 8], 10, true);
    //clapp and his settings
    villain = game.add.sprite(10,10, 'clapp');
   
    //enables physics on the villain
    game.physics.arcade.enable(villain);
    //giving the villain the same physics properties.
    villain.body.bounce.y = 0.2;
    villain.body.gravity.y = 300;
    villain.body.collideWorldBounds = true;
   
    //animations for walking left and right
    villain.animations.add('clappRight', [5, 6, 7, 8], 10, true);
    villain.animations.add('clappLeft', [4, 3, 2, 1], 10, true);
   
    //  Finally some clues to collect
    clues = game.add.group();
    //  We will enable physics for any clue that is created in this group
    clues.enableBody = true;
    //  Here we'll create 12 of them evenly spaced apart
      for (var i = 0; i < 60; i++)
    {
        //  Create a clue inside of the 'clues' group
        var clue = clues.create(i * 250, 0, 'clue');
        //  Let gravity do its thing
        clue.body.gravity.y = 300;
        //  This just gives each clue a slightly random bounce value
        clue.body.bounce.y = 0.7 + Math.random() * 0.2;
    }
   
    //  The score
    scoreText = game.add.text(16, 16, 'score: 0', { fontSize: '32px', fill: '#fff' });
    scoreText.fixedToCamera = true;
    //  Our controls.
    cursors = game.input.keyboard.createCursorKeys();
   
    //Creates the variable for the timer based on game time
//    timer = game.time.create(false);
//    
//    //Places the timer text at 550 pixels from left, 16 pixels from top, and sets the same font size and colour as the Scores
//    timerText = game.add.text(550, 16, 'Time: 00:00', { fontSize: '32px', fill: '#fff'});
//    
//    //Stops the timer scrolling with the background
//    timerText.fixedToCamera = true;
//    
//    //Sets up a countdown loop 65,000 milliseconds long (eg 65 seconds)
//  //  timer.loop(65000, gameEnd, this);
//
//    //Starts the timer (so could be linked to the outcome of another function, or a “start” button)
//    timer.start();
    }
function update() {
    //  Collide the player and the clues with the platforms
    game.physics.arcade.collide(player, platforms);
    game.physics.arcade.collide(clues, platforms);
    game.physics.arcade.collide(villain, platforms);
    //  Checks to see if the player overlaps with any of the clues, if he does call the collectClue function
    game.physics.arcade.overlap(player, clues, collectClue, null, this);
   
   
    //checks to see if the villain overlaps the player, then activates clappEat
    game.physics.arcade.overlap(villain, player, clappEat, null, this);
    //  Reset the players velocity (movement)
    player.body.velocity.x = 0;
    if (cursors.left.isDown)
    {
        //  Move to the left
        player.body.velocity.x = -150;
        player.animations.play('left');
    }
    else if (cursors.right.isDown)
    {
        //  Move to the right
        player.body.velocity.x = 150;
        player.animations.play('right');
    }
    else
    {
        //  Stand still
        player.animations.stop();
        player.frame = 4;
    }
   
    //  Allow the player to jump if they are touching the ground.
    if (cursors.up.isDown && player.body.touching.down)
    {
        player.body.velocity.y = -350;
    }
   
    if (game.input.keyboard.isDown(Phaser.Keyboard.LEFT))
    {
        game.camera.x -= camSpeed;
       // if (!game.camera.atLimit.x)
      //  {
        //    s.tilePosition.x += camSpeed / 10;
       //c.tilePosition.x += camSpeed / 10; }
    }
    else if (game.input.keyboard.isDown(Phaser.Keyboard.RIGHT))
    {
        game.camera.x += camSpeed;
      //  if (!game.camera.atLimit.x)
       // {
       //     s.tilePosition.x -= camSpeed / 10;
       //     c.tilePosition.x -= camSpeed / 10;}
    }
    if (player.body.x < villain.body.x - 48)
        {
            villain.body.velocity.x = -100;
            villain.animations.play('clappLeft');
        }
     else if (player.body.x > villain.body.x + 48)
        {
            villain.body.velocity.x = 100;
            villain.animations.play('clappRight');
        }
     else
        {
            villain.animations.stop();
            villain.frame = 4;
        }
   
    if (villain.body.touching.down)
        {
        villain.body.velocity.y = -350;
        }
    function collectClue (player, clue) {
   
    // Removes the clue from the screen
    clue.kill();
    //  Add and update the score
    score += 1;
    scoreText.text = 'Clues: ' + score;
   
    }    
   
    function clappEat (villain, player){
   
    //kills the player
    player.kill();
   
    //says text
    scoreText.text = 'You died! You were killed by the serial killer.';
   
    //reduces score to 0
    score = 0;
    }
   
    //updateTimer()
   
//    function updateTimer() {
//    minutes = Math.floor(game.time.time / 60000) % 60;
//    seconds = Math.floor(game.time.time / 1000) % 60;
//    //If any of the digits becomes a single digit number, pad it with a zero
//    if (seconds < 10)
//        seconds = '0' + seconds;
//    if (minutes < 10)
//        minutes = '0' + minutes;
// timerText.text = 'Time: ' + minutes + ':'+ seconds;
//    }
   
   
}
</script>
<div id="phasergame"></div>
</body>
</html>
~~~~

## Final Game Code

I made adjustments to the code so that I there where elements of randomness to keep the player guessing. When the player crosses the debt collector, who now spawns randomly, the debt collector can now slightly change his speed. His jumps are also now randomised so as to keep the player guessing and more engaged in the game.

I also made changes to how the player character could move. Initially, the jump was more than enough to cover the office supply obstacles, and I felt this made the game too easy for the player. So I made the jump have a lower peak, requiring the players jumps to be more skilful to survive.

The money also spawns randomly on the map and reset when they've all been collected. This is so the game is more interesting and can also last longer for the player.

Below is all changes made from above.

~~~~
<!doctype html>
<html lang="en">
<head>
    <title>Game</title>




    game.load.image('city', 'Assets/city.png');
    game.load.image('car1', 'Assets/car1.png');
    game.load.image('car2', 'Assets/car2.png');





function createClues() {

    for (var i = 0; i < 60; i++)
{
        //  Create a clue inside of the 'clues' group
        var clue = clues.create(4000*Math.random(), 0, 'clue');

        //  Let gravity do its thing
        clue.body.gravity.y = 900;

        //  This just gives each clue a slightly random bounce value
        clue.body.bounce.y = 0.2 + Math.random() * 0.2;
}

}





    game.add.tileSprite(0, 0, 4000, 700, 'city');








    //var ledge = platforms.create(-3960, 200, 'ground');
    //ledge.body.immovable = true;

    var barrier = platforms.create(400, 500, 'car1');

    var barrier = platforms.create(1700, 500, 'car1');

    var crate = platforms.create(1100, 500, 'car2');

    var crate = platforms.create(2900, 500, 'car2');

    var crate = platforms.create(3400, 500, 'car2');



    player.body.gravity.y = 600;


    villain = game.add.sprite(Math.random()*4000,10, 'clapp');



    villain.animations.add('clappLeft', [0, 1, 2, 3], 10, true);



        createClues();


    scoreText = game.add.text(16, 16, 'Money: $0', { fontSize: '32px', fill: '#fff' });


//
//
//

//villain.body.x=Math.random()*4000;


    console.log(villain.body.x);















            villain.body.velocity.x = -100+Math.random()*-70;

            villain.body.velocity.x = 100+Math.random()*70;


        villain.body.velocity.y = -150+Math.random()*-300;






    score += 10;
    scoreText.text = 'Money: $' + score;

        if (score %600==0){

            createClues();

        }

    }



    scoreText.text = 'Gotcha. The Debt collector caught you';

