# Group-02
## Coriolis Force simulation

## I. Members
1. 楊添福 106022106
2. 謝明儒 106022215
3. 鄭銘健 106022162
4. 黃景岳 106022225

## II. Goals
To simulate the movement of a throwing ball on a rotating system. (eg. Earth)

## III. Steps
1. Depicting the ball and the rotating planet at initial condition.

2. Calculate the Coriolis force acting on the ball.

3. Present the simulation by a video.

## IV. Assignment
1. 楊添福 -> Formulate the Coriolis force acting on the ball.

2. 黃景岳 -> Find out the solution of the movement equation.

3. 謝明儒 -> Export the animation of the throwing ball.

4. 鄭銘健 -> Debug and check thework of vpython code.

## V. Brief Introduction
Coriolis force is an inertial force acting on objects that are in motion within a frame of reference which rotating with respect to an inertial frame. In a reference frame with clockwise rotation mode, the force acts to the left of the motion of the object; in one with counterclockwise rotation mode, the force acts to the right of the motion of the object.

## VI. Work Log
Week 1:
1. Discuss and organize our final project topic and goal.

Week 2:
1. Install vpython and look out the difference of it.
2. Formulate the motion of the throwing ball.
3. Solve the equation and observe how it moves.

Week 3:
1. Define all the constants we need in this project.
2. Set up input format, containg latitude, velocity, and duration.
3. Generate two windows to show our later detail works.

Week 4:
1. Import the figure of Earth to be as our rotating system.
2. Let the Earth rotates at constant speed and connects to the another window.
3. Set the original frame of observer to be from the outer space.

Week 5:
1. Fire a ball and draw the trajectory 3-D on the main window and the other.
2. Set up a control to save all the datas to check whether it's correct or not.
3. Set a circular plane as the Great circle where the throwing ball only due to force of gravity.

Week 6:
1. Continue our work last week and connects the data simutaneously with another window.
2. Advance our code in order to fire the ball more than one time.
3. Add some extra controls, such as changing fire angle and the frame of observer.

Week 7:
1. Check our progress whether it suits the reality.
2. Beatify our video and refine our vpython code.

Week 8:
1. Double check our code and finish our github and PPT.

## VII. Progress
We first type the ball's information to be latitude: 30 degree, initial velocity: 25000 meter per second, and duration: 3 seconds. After few seconds, we'll get two windows, one with Earth and ball's information at the left side, and the other showing where the ball will land after firing. We click the main window, and see the Earth start to rotate at fixed velocity. We click again and observe that a firing ball due to not only gravity but Coriolis force fly northward initially. Ultimately, it lands at the right side of the firing direction, namely eatward. If we want to fire another ball with dirrerent information, such as direction and angle, we just need to press our keyboard and then click again. The results fit the theory and we successfully simulate the movement of a throwing ball on a rotating system.

## VIII. Final Presentation PPT
https://onedrive.live.com/edit.aspx?cid=f519f694535a8eae&page=view&resid=F519F694535A8EAE!109708&parId=F519F694535A8EAE!103&app=PowerPoint

## IX. Coding
The whole coding is in below:
https://docs.google.com/document/d/15DKHIShpjO1bCL_Bxtbdo0cloy_CmjVOFSsGbObxY4M/edit?fbclid=IwAR3h6AwNaqk8psE6l9WQL7x7no1TpTmX0ZsGgTP7jI5izmfDrwNB07g3SbU

We used Vpython to run the code, make sure install Vpython and Python 2.7 before run the code
