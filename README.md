# Assignment-Research Track 1 (Python Robotics Simulator)

Abstract 
================================

The concept of this assignment is to develop a Python-language code to simulate a robot circulating in a plant, in the meantime it has to pick up the silver tokens and put it behind it and if it encounters the golden tokens it has to avoid touching it and go back or change direction. According to this code, you have to understand how to use the medotics R. see (), R. grab (), R. release () and how to use find_token () to find out what kind of token is in front of robot.

Introduction
================================
This python simulator was already programmed by _Harry Cutts_ in 2017, here is reported in a sort of assignment for Reseach Track 1 course of the first year Robotic Engineering. Assignment is modified from the original model by prof. _Carmine Tommaso Recchiuto_ is compiled in python brought to linux in 3 formats by exercises. To compile assignment you only need to modify exercises 2 and 3 to get desired results.

Materials and Methods
=========================
Before compiling program you have to install it and running on the basic program explained in the part _Installing and running_ ,in case after installing there have been any problems before programming you can refer apart _Troubleshooting_ and for program execution you can see part _Execution_. An imporatnte challenge in this job is that robotic sensors can detect boxes around all directions (from -180.0 degrees to 180.0 degrees). So you have to develop a python code first to know the type of token, to do this two functions __def find_token_silver()__ and __def find_token_golden()__ have been defined in code, which are in order to find the silver token and golden token. Methods and conditions have been defined within each function, one of which is the __R.see()__ method which is used within a __for()__ loop. R is going to say Robot() if it sees a token. and with another method which is __token.info.marker_type__ knows it type of token. Consequently I defined in a __while__ loop conditions by chance to meet silver token or golden token which to understand better I did a flwochart, and if you encounter silver token you have to grab it with the __grab()__ method and release it in the back if with the __release()__ method and continue to run in the plan without touching golden token and find more silver tokens.

Installing and running
----------------------
The simulator requires a Python 2.7 installation, the [pygame](http://pygame.org/) library, [PyPyBox2D](https://pypi.python.org/pypi/pypybox2d/2.1-r331), and [PyYAML](https://pypi.python.org/pypi/PyYAML/).

Pygame, unfortunately, can be tricky (though [not impossible](http://askubuntu.com/q/312767)) to install in virtual environments. If you are using `pip`, you might try `pip install hg+https://bitbucket.org/pygame/pygame`, or you could use your operating system's package manager. Windows users could use [Portable Python](http://portablepython.com/). PyPyBox2D and PyYAML are more forgiving, and should install just fine using `pip` or `easy_install`.

## Troubleshooting

When running `python run.py <file>`, you may be presented with an error: `ImportError: No module named 'robot'`. This may be due to a conflict between sr.tools and sr.robot. To resolve, symlink simulator/sr/robot to the location of sr.tools.

On Ubuntu, this can be accomplished by:
* Find the location of srtools: `pip show sr.tools`
* Get the location. In my case this was `/usr/local/lib/python2.7/dist-packages`
* Create symlink: `ln -s path/to/simulator/sr/robot /usr/local/lib/python2.7/dist-packages/sr/`

## Execution

To run one or more scripts in the simulator, use `run.py`, passing it the file names. 

I am proposing you three exercises, with an increasing level of difficulty.
The instruction for the three exercises can be found inside the .py files (exercise1.py, exercise2.py, exercise3.py).

When done, you can run the program with:

```bash
$ python run.py exercise1.py
```

You have also the solutions of the exercises (folder solutions)

```bash
$ python run.py solutions/exercise1_solution.py
```
-------------------------------------------
__Functions already developed in code(This part has been paired without any modifications)©__
--------------------------------------
Robot API
---------

The API for controlling a simulated robot is designed to be as similar as possible to the [SR API][sr-api].

### Motors ###

The simulated robot has two motors configured for skid steering, connected to a two-output [Motor Board](https://studentrobotics.org/docs/kit/motor_board). The left motor is connected to output `0` and the right motor to output `1`.

The Motor Board API is identical to [that of the SR API](https://studentrobotics.org/docs/programming/sr/motors/), except that motor boards cannot be addressed by serial number. So, to turn on the spot at one quarter of full power, one might write the following:

```python
R.motors[0].m0.power = 25
R.motors[0].m1.power = -25
```

### The Grabber ###

The robot is equipped with a grabber, capable of picking up a token which is in front of the robot and within 0.4 metres of the robot's centre. To pick up a token, call the `R.grab` method:

```python
success = R.grab()
```

The `R.grab` function returns `True` if a token was successfully picked up, or `False` otherwise. If the robot is already holding a token, it will throw an `AlreadyHoldingSomethingException`.

To drop the token, call the `R.release` method.

Cable-tie flails are not implemented.

### Vision ###

To help the robot find tokens and navigate, each token has markers stuck to it, as does each wall. The `R.see` method returns a list of all the markers the robot can see, as `Marker` objects. The robot can only see markers which it is facing towards.

Each `Marker` object has the following attributes:

* `info`: a `MarkerInfo` object describing the marker itself. Has the following attributes:
  * `code`: the numeric code of the marker.
  * `marker_type`: the type of object the marker is attached to (either `MARKER_TOKEN_GOLD`, `MARKER_TOKEN_SILVER` or `MARKER_ARENA`).
  * `offset`: offset of the numeric code of the marker from the lowest numbered marker of its type. For example, token number 3 has the code 43, but offset 3.
  * `size`: the size that the marker would be in the real game, for compatibility with the SR API.
* `centre`: the location of the marker in polar coordinates, as a `PolarCoord` object. Has the following attributes:
  * `length`: the distance from the centre of the robot to the object (in metres).
  * `rot_y`: rotation about the Y axis in degrees.
* `dist`: an alias for `centre.length`
* `res`: the value of the `res` parameter of `R.see`, for compatibility with the SR API.
* `rot_y`: an alias for `centre.rot_y`
* `timestamp`: the time at which the marker was seen (when `R.see` was called).

For example, the following code lists all of the markers the robot can see:

```python
markers = R.see()
print "I can see", len(markers), "markers:"

for m in markers:
    if m.info.marker_type in (MARKER_TOKEN_GOLD, MARKER_TOKEN_SILVER):
        print " - Token {0} is {1} metres away".format( m.info.offset, m.dist )
    elif m.info.marker_type == MARKER_ARENA:
        print " - Arena marker {0} is {1} metres away".format( m.info.offset, m.dist )
```

[sr-api]: https://studentrobotics.org/docs/programming/sr/

Results
==============
The results obtained are brought in the form of images to understand better.

![Immagine](https://user-images.githubusercontent.com/80394968/139293917-306d8437-15b6-47ee-af65-8fac06ac4eec.png)
_Fig.1:_ Is demonstrated distance longitudinal and lateral that robot can know and control relative golden token not to go against.

--------------------

![2](https://user-images.githubusercontent.com/80394968/139299852-5d0e0d98-c3ee-4b0e-88d0-134f92392723.png)
_Fig2:_ Check longitude and lateral distance from the center of the robot to find silver token

--------------------

![3](https://user-images.githubusercontent.com/80394968/139299885-8d42c584-e03d-4178-86c6-ea467b73d34f.png)
_Fig3:_ Check angle with respect to silver token and orient it towards larger 
