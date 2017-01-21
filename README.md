This program runs the game Flappy Bird and uses Q-learning to learn a path.
I only wrote the Q-learning related parts, altering the Flappy Bird game
originally written by TimoWilken.
                                                                         
                                                                         
**How to run the program:**                                                                       
Pygame is required to run the code. When you run this program, the compiler
will complain about a depricated audio unit, but the program should still run.
To run a new bird in the training process (note: you can run it with python 2.7,
which is considorablyfaster, though the code is written for python 3):                                                  
python3 flappybird.py                                                                         

To run the already trained bird:                                                            
python3 flappybird.py trained.pkl
                                                                                                                      
                                                                                                   
**Sources Consulted:**                                                                       
https://github.com/TimoWilken/flappy-bird-pygame
                                                                         
                                                                         
**Q-learning Details**                                                                       
Instead of exiting after the game ends, the program loops and plays the game
100 times. At the end of the 100 times, the program saves the bird's training
to the specified file. It usually reaches 10 or more pipes, sometimes as many
as 50 and other times as few as 0. It has not fully converged yet, as that would
have taken quite a long time. It still reaches a good amount of pipes at this
stage.

Each state used in the Q-learning algorithm corresponds to a key in a hash
table. The hash table stores the reward for each action (going up and doing
nothing) and a value used to compute the learning rate. This allowed me to
decrease the learning rate corresponding to how many times each state is
visited, though at a cost of needing to store 50% more values.

In creating the states for the bird's Q-learning, I chose an interval of 9 for
updating the bird's next Q-learning state. There were two reasons for this:
to mimic human reaction time and limit the number of possible states. First of
all, a human cannot click the bird every time the bird's position is updated
because we just are not that quick. Second, I chose to include the bird's
slope in the Q-learning states because if the bird tries to fly up when it has
a slope of -1, it goes up far more quickly than when it has a slope of 1.7. The
bird moves in the motion of a sine curve, which takes 18 updates to complete.
The number 9 divides 18 evenly, so only two slopes for the bird are ever seen in
the state representation.

In addition to the bird's slope, I also have the bird's horizontal distance from
the pipe opening and the bird's vertical distance from the pipe opening in the
Q-learning state representation. The bird survives by flying through the pipes,
so the distance and what the bird does at each point is what keeps the bird
alive. I saw that the bird updates by 3's, so I divided the x and y by 3.

After watching the bird learn for a while, its tactic for passing the pipes
close to the bottom of the screen seemed to conflict with its tactic for passing
the pipes near the top of the screen, which seemed to limit the number of pipes
the bird passed without crashing. I decided to try adding the bird's y value
divided by half the window's height (256) to the state representation. This did 
allow the bird to live longer.

In choosing an exploratory function, I decided to make the bird explore if it
is the first or second time the state has been visited. This was possible using
the same saved data used for the learning rate. When exploring, the bird goes up
50% of the time and does nothing the other 50%.
