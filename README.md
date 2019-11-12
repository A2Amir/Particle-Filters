# Particle Filters
 In this section, you will learn about particle filters for estimating the state of a system
 
 
In this section, you will learn about particle filters. In our sequence of algorithms () for estimating the state of a system, this is the third one and in many ways is the best one because:

* It's the easiest to program 
* In most ways is the most flexible. 

The state space for particle filters is usually continuous and we're not confined to unimodal distributions We can actually represent arbitrarily multimodal distributions but the key advantage is they're really easy to program. 
See a particle filter in action. Below is a floor plan of an environment where a robot is located and it has to perform what is called global localization, it has no clue where it is and it has to find out where it is just based on sensor measurements. 

This Robot has range sensors as indicated by the blue stripes, the Robot uses sonar sensors, which means sound, to range the distance of nearer obstacles and it must use these distance sensors to determine a good posterior distribution of where it is, what the robot doesn't know. In fact, it's completely uncertain where it is. 

Now, the particle filter is represented using particles. Each of these red dots (below figure) of which there are several thousand here is a discrete guess where the robot might be. It's structured as a X coordinate, a Y coordinate, and also a heading direction and these 3 values together comprise a single guess, but a single guess is not a filter.
A filter is the set of several thousands of such guesses that together comprise an approximate representation for the posterior of the robot. 

<p align="right"> <img src="./img/1.jpg" style="right;" alt=" particle filters" width="600" height="400"> </p> 
