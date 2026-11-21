# gazebo-myrobot

My work for the **Gazebo Basics** lesson of Udacity's Robotics Software Engineer Nanodegree (Course 2: Gazebo World).

It contains a two-wheeled robot built in Gazebo's Model Editor, a world that holds it, a world plugin that prints `Hello World!`, and a house for the robot designed in the Building Editor.

## Layout

```
myrobot/
├── model/
│   ├── robot/        # two-wheeled robot: box chassis + two cylinder wheels on revolute joints
│   └── building/     # U-shaped brick house: 10 m x 10 m, 2.5 m wide wings, one door, two windows
├── world/
│   └── myworld       # empty world + the robot + the hello plugin
├── script/
│   └── hello.cpp     # WorldPlugin that prints "Hello World!"
└── CMakeLists.txt    # builds script/hello.cpp into libhello.so
```

## Run it

The lesson keeps everything in `/home/workspace/myrobot`, so clone it there:

```sh
sudo mkdir -p /home/workspace && sudo chown $USER /home/workspace
git clone https://github.com/vivianesolomon/gazebo-myrobot.git /home/workspace/myrobot
```

Build the plugin:

```sh
cd /home/workspace/myrobot
mkdir build && cd build
cmake ..
make
export GAZEBO_PLUGIN_PATH=${GAZEBO_PLUGIN_PATH}:/home/workspace/myrobot/build
```

Launch the world. `Hello World!` is printed in the terminal and the robot appears in the scene:

```sh
cd /home/workspace/myrobot/world
gazebo myworld
```

If the plugin does not load, run `gazebo myworld --verbose` and check that `GAZEBO_PLUGIN_PATH` includes the `build` directory.

## Add the house

To make the models show up under Gazebo's **Insert** tab, add the model folder to the model path before launching:

```sh
export GAZEBO_MODEL_PATH=${GAZEBO_MODEL_PATH}:/home/workspace/myrobot/model
```

Then pick `building` (or `robot`) from the Insert tab and click in the scene to place it.

## Robot dimensions

| Part    | Shape    | Size                         | Pose                         |
|---------|----------|------------------------------|------------------------------|
| Chassis | Box      | 0.3 x 1.0 x 0.1 m            | z = 0.2                      |
| Wheels  | Cylinder | radius 0.2 m, length 0.1 m   | z = 0.2, roll = 1.5707 rad   |

Each wheel is attached to the chassis with a revolute joint about the wheel's Z axis.
