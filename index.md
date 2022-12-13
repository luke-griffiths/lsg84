# Microcontrollers Final Project: Dino Jump Video Game

## Introduction
I created a joystick-based video game using an IMU, microcontroller, and VGA monitor. Why? Because who doesn't like old-fashioned, arcade-style video games. The game has basic features of an arcade game: a cool welcome screen, pixelated graphics, a leaderboard that keeps track of high scores, pause/resume functionality, and a joystick to control the main character. The goal: get as high as possible and avoid getting hit by cannonballs. 

## High Level Design
Dino Jump is very similar to the Doodle Jump app, except the graphics are more reminiscent of a retro arcade game, and the main character is a cool dinosaur named Ava. 
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
I trust that the reader can figure out what each of those states corresponds to. 

*insert FSM*

### Landing Pads

### IMU

### Threading & Concurrency



## Program/Hardware Design

## Results

## Conclusions


## Appendix

I approve this report for inclusion on the course website.
I approve the video for inclusion on the course youtube channel.
