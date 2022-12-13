# Microcontrollers Final Project: 
# Dino Jump Video Game

## Introduction
I created a joystick-based video game using an IMU, microcontroller, and VGA monitor. Why? Because who doesn't like old-fashioned, arcade-style video games. The game has basic features of an arcade game: a cool welcome screen, pixelated graphics, a leaderboard that keeps track of high scores, pause/resume functionality, and a joystick to control the main character. The goal: get as high as possible and avoid getting hit by cannonballs. 

## High Level Design
Dino Jump is very similar to the Doodle Jump app, except the graphics are more reminiscent of a retro arcade game, and the main character is a cool dinosaur named Ava. 




## Program/Hardware Design
### Hardware
The hardware setup for this project was simple. Everything connected to the RP2040 microcontroller via the Pico interface board. A picture is worth a thousand words, so here is how to setup the hardware.

<img width="571" alt="Screen Shot 2022-12-12 at 10 42 44 PM" src="https://user-images.githubusercontent.com/71809396/207221212-2aa24a23-33fa-4d6d-9554-5b4193673aac.png">

### Game Kinematics
Ava (the dinosaur) moves according to the laws of physics. Her position is updated according to the projectile motion equations you learned in high school. Of course, as in all good physical simulations, I ignored air resistance. Finding values for Ava's jump velocity and gravity that worked well with the game's frame rate of 30fps AND looked realistic was challenging. 
```
/* updates Ava's (x,y) and (vx, vy) according to projectile motion */
void dinoKinematics(){
  avavx = complementary_angle;
  if (avavx > int2fix(SPEED_LIMIT))
    avavx = int2fix(SPEED_LIMIT);
  else if (avavx < int2fix(SPEED_LIMIT))
    avavx = int2fix(-1 * SPEED_LIMIT);

  avavy += multfix(multfix(g, int2fix(time_kinematics)) + jump_y_vel;
  avax += multfix(int2fix(10), avavx);
  avay += multfix(half_g, int2fix(time_kinematics * time_kinematics)) + jump_y_vel;
}
```
In that block of code, you'll notice that I restrict the maximum speed Ava can move along the x-axis. ```complementary_angle``` is a global variable (gasp) that holds the most recent value reported by the IMU. You'll also notice that her y-velocity is updated according to projectile motion, where ```time_kinematics``` is a variable that increments every frame and resets every time Ava jumps. The last thing you should notice is that almost every kinematics computation uses fixed point operations on fixed point numbers. This is a huge savings in computation because fixed point operations are much faster than floating point. The update kinematics function for cannonballs is similar, so I won't include it here. In addition, a third function moves EVERYTHING in the environment down, to give the illusion that Ava is jumping to higher ground. 

### Game FSM
The game had these states: 
```
enum states{
  WELCOME,
  LEADERBOARD,
  PLAY,
  PAUSE,
  GAMEOVER
} state = WELCOME;
```
I trust that the reader can figure out what each of those states corresponds to. User input from the connected keyboard is relayed to the RP2040 and switches the state. The game automatically moves from state ```PLAY``` to ```GAMEOVER``` when the game is lost. 

<img width="587" alt="Screen Shot 2022-12-12 at 10 53 19 PM" src="https://user-images.githubusercontent.com/71809396/207222544-6c3713f4-c1d8-432f-bb5d-44524cc8fcff.png">

### Landing Pads

### IMU

### Threading & Concurrency

## Results
Here's a video of the game in action.

<iframe width="560" height="315" src="https://www.youtube.com/embed/TZojMDk_-_0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

There are two main flaws with the current design: 
1. Flicker. I was unable to figure out what caused the dinosaur to occasionally flicker when it moves quickly. You can see this when the dinosaur jumps or falls quickly, and you can see it in the pads and dinosaur when the environment shifts down. 
2. Bounding box collision detection. Since I detect collisions with pads (and cannonballs) via a bounding box drawn around the dinosaur, you can have the dinosaur's feet be in free space but the dinosaur is still "on a pad" since a line drawn down from the dinosaur's snout *would be* on the pad. In addition, when the dinosaur moves very fast (like when in free fall) it could move so fast in one frame that it goes right through a pad. This rarely happens, but is possible. The fix would be to "add air resistance" so the dinosaur would have a terminal velocity slow enough to never travel the distance of a pad in one frame. 

## Conclusions
Overall this project was a success. The game is very playable, it looks good, and working on it made me happy. In addition, I had plenty of "spare time" to do additional computations while maintaining 30fps. This means that
1. my code is efficient
2. I have the ability to add in cool features and the frame rate won't be affected

One feature that I would have liked to add was sound effects. Most arcade games that I know of have funny sound effects, and it wouldn't have been too difficult to add a *boing* noise every time Ava jumped. Ultimately, like any software project, there is a tradeoff between deadlines and feature development. Unfortunately, the sound effect feature was a casualty of this tradeoff. 

As far as acknowledgements, nearly all the code written was my own. I did re-use my own complementary filter code for the IMU from Lab 3, and I used Hunter's VGA graphics library and several Pico hardware libraries for i2c and pio. Thank you to Hunter and Bruce for providing debugging suggestions and for giving me the knowledge necessary to complete this assignment!

## Appendix
I approve this report for inclusion on the course website.
I approve the video for inclusion on the course youtube channel.
