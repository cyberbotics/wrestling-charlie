# Humanoid Robot Wrestling Controller Example

[![webots.cloud - Competition](https://img.shields.io/badge/webots.cloud-Competition-007ACC)][1]

## Charlie controller

Demonstrates how to access the LEDs and play a custom motion file.

Beats [Bob](https://github.com/cyberbotics/wrestling-bob) by moving forwards and pushing him to the ground.

This controller is a simple example of how to use the Motion_library class from the [motion.py](./controllers/utils/motion.py) module to add a custom motion file, here [Shove.motion](./controllers/participant/Shove.motion).

Here is the [participant.py](./controllers/participant/participant.py) file:

``` Python
from controller import Robot
import sys
sys.path.append('..')
from utils.motion_library import MotionLibrary


class Charlie (Robot):
    def __init__(self):
        Robot.__init__(self)
        self.time_step = int(self.getBasicTimeStep())

        # there are 7 controllable LEDs on the NAO robot, but we will use only the ones in the eyes
        self.leds = {
            'right': self.getDevice('Face/Led/Right'),
            'left':  self.getDevice('Face/Led/Left')
        }

        self.library = MotionLibrary()
        # adding a custom motion to the library
        self.library.add('Shove', './Shove.motion', loop=True)

    def run(self):
        self.library.play('Stand')

        self.leds['right'].set(0xff0000)  # set the eyes to red
        self.leds['left'].set(0xff0000)

        while self.step(self.time_step) != -1:
            # When the robot is done standing for stabilization, it moves forwards
            if self.library.get('Stand').isOver():
                self.library.play('ForwardLoop')  # walk forward
                self.library.play('Shove')        # play the shove motion


# create the Robot instance and run main loop
wrestler = Charlie()
wrestler.run()
```

[David](https://github.com/cyberbotics/wrestling-david) is a more advanced robot controller able to win against Charlie.

[1]: https://webots.cloud/run?version=R2022b&url=https%3A%2F%2Fgithub.com%2Fcyberbotics%2Fwrestling%2Fblob%2Fmain%2Fworlds%2Fwrestling.wbt&type=competition "Leaderboard"
