# Particle Filters
 In this section, you will learn about particle filters for estimating the state of a system
 
## Introduction

In this section, you will learn about particle filters. In our sequence of algorithms () for estimating the state of a system, this is the third one and in many ways is the best one because:

* It's the easiest to program 
* In most ways is the most flexible. 

The state space for particle filters is usually continuous and we're not confined to unimodal distributions We can actually represent arbitrarily multimodal distributions but the key advantage is they're really easy to program. 
See a particle filter in action. Below is a floor plan of an environment where a robot is located and it has to perform what is called global localization, it has no clue where it is and it has to find out where it is just based on sensor measurements. 

This Robot has range sensors as indicated by the blue stripes, the Robot uses sonar sensors, which means sound, to range the distance of nearer obstacles and it must use these distance sensors to determine a good posterior distribution of where it is, what the robot doesn't know. In fact, it's completely uncertain where it is. 

Now, the particle filter is represented using particles. Each of these red dots (below figure) of which there are several thousand here is a discrete guess where the robot might be. It's structured as a X coordinate, a Y coordinate, and also a heading direction and these 3 values together comprise a single guess, but a single guess is not a filter.
A filter is the set of several thousands of such guesses that together comprise an approximate representation for the posterior of the robot. 

<p align="right"> <img src="./img/1.jpg" style="right;" alt=" particle filters" width="600" height="400"> </p> 
In the beginning of the performing of the particle filter, particles are uniformly spread, 
but the particle filter makes them survive in proportion of how consistent one of these particles is with a sensor measurement. Very quickly the robot has figured out it's in the corridor, but 2 clouds survive because of the symmetry of the corridor (see below figure). 

<p align="right"> <img src="./img/2.jpg" style="right;" alt=" particle filters" width="600" height="400"> </p> 

As the robot enters one of the offices, the symmetry is broken and the correct set of particles survive (below figure). 

<p align="right"> <img src="./img/3.jpg" style="right;" alt=" particle filters" width="600" height="400"> </p> 

The essence of particle filters is to have the particles guess, where the robot might be moving but also have them survive using effectively survival of the fittest so that particles that are more consistent with the measurements are more likely to survive and as a result places of high probability will collect more particles and therefore be more representative of the robot's posterior belief. Those particles together(those thousands of particles) are now clustered in a single location(see above figure). Those comprise the approximate belief of the robot as it localizes itself. 


## Using Robot Class

A piece of code is written to demonstrate a robot class. The main class is a class called robot. This robot lives in a 2-dimensional world of size 100 meters. It can see 4 different landmarks that are located at the following coordinates: 20, 20; 80,80; 20,80; 80,20.

Noise filters are really important for particle filters. To assimilate noise, the robot class has a function that allows you to set them, but the noise filters are all now set to 0 and you can play with them if you want. 
The robot class has also a function that is important as we implement particle filters called measurement_prob, it accepts a measurement and tells you how plausible the measurement is (the key thing for the survival of the fittest rule in particle filters)

 
Below is how we make such a robot. It's really easy. All you have to do is call a function robot() and assign it to a variable myrobot. Now that we can do things with myrobot. For example, we can set a position. These 3 values are the X coordinate, the Y coordinate, and the heading in radians.
~~~python
myrobot = robot()
myrobot.set(10,10,0.0)
print (myrobot)
~~~ 
[x=10.0 y=10.0 orient=0.0]

We can make the robot move. This robot moves 10 meters forward and doesn't turn. 
So, let's print the resulting position. 

~~~python
myrobot = robot()
myrobot.set(10,10,0.0)
print(myrobot)
myrobot=myrobot.move(0.0,10)
print(myrobot)
~~~ 
[x=10.0 y=10.0 orient=0.0]

[x=20.0 y=10.0 orient=0.0]


let's make the robot turn by pi/2 and move 10 meters. So, now the robot is heading in the direction of pi/2(North), and it moved forward 10 meters in the Y direction, instead of the X direction. 
~~~python
myrobot = robot()
myrobot.set(10,10,0.0)
print(myrobot)
myrobot=myrobot.move(pi/2,10)
print(myrobot)
~~~ 

[x=10.0 y=10.0 orient=0.0]

[x=10.0 y=20.0 orient=1.5707]



The last thing I want to show is how to generate measurements. There is a really easy command called sense() and all it does is give you the distance to the 4 landmarks.

~~~python
myrobot = robot()
myrobot.set(10,10,0.0)
print(myrobot)
myrobot=myrobot.move(pi/2,10)
print(myrobot)
print (myrobot.sense())
~~~ 

[x=10.0 y=10.0 orient=0.0]

[x=10.0 y=20.0 orient=1.5707]

[10.0, 92.19544457292888, 60.8276253029822, 70.0]

## Robot World

Now we know about the class robot, who can turn and then move straight after the turn, and it also can sense the distance to 4 designated landmarks, L1, L2, L3, and L4, and these distances comprise the measurement vector of the robot. 

I told you the robot lives in a world of size 100 x 100 and if this robot falls off one end, it appears on the other. it's a cyclic world. 

<p align="right"> <img src="./img/4.jpg" style="right;" alt=" Robot World" width="600" height="400"> </p> 

## Creating Particles

The particle filter that we are going to program maintains a set of 1000 random guesses (red dots) as to where the robot might be. Each of the red dots is a vector which contains an X coordinate, in this case 38.2, a Y coordinate 12.4 and a heading direction of 0.1, which is the angle at which the robot points relative to the X axis (see below figure). 


<p align="right"> <img src="./img/5.jpg" style="right;" alt=" Particles " width="600" height="400"> </p> 

to make a particle set of 1000 particles, every time we call the function robot and assign it to a list called p[],the elements of the p[] are x, y, and orientation which are initialized at random. 

~~~python
N=1000
p=[]
for i in range(N):
    x=robot()
    p.append(x)
print(len(p))
~~~ 
100

I now want to take each of these particles and simulate robot motion. Depending on the heading direction, this might yield a different direction. So, each of these particles shall first turn by 0.1 and then move by 5 meters (below figure).

<p align="right"> <img src="./img/6.jpg" style="right;" alt=" Creating Particles and move " width="600" height="400"> </p> 

~~~python
N=1000
p=[]
for i in range(N):
    x=robot()
    p.append(x)
print(len(p))
p2=[]
for i in range(N):
    p2.append(p[i].move(.1,5.0))
p=p2

~~~ 
I got about half of particle filters implemented, unfortunately it's the easy half, but the difficult half isn't that much more difficult.


