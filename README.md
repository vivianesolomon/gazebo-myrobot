# gazebo-myrobot

My work for Course 2 (Gazebo World) of Udacity's Robotics Software Engineer Nanodegree:

- **[Build My World](#build-my-world)**, the course project, in `build_my_world/`.
- The **Gazebo Basics** lesson exercises, in the rest of the repo: a two-wheeled robot built in Gazebo's Model Editor, a world that holds it, a world plugin that prints `Hello World!`, and a house for the robot designed in the Building Editor.

## Build My World

A Gazebo world of my apartment, modeled at true size, with two copies of a four-wheeled robot inside it. A world plugin prints `Welcome to Viviane's World!` when the world launches.

```
build_my_world/
├── model/
│   ├── apartment/        # Building Editor: single-floor 1-bedroom, 5.9 m x 7.9 m, tan walls, 4 doors, windows
│   └── my_robot/         # Model Editor: box chassis + four cylinder wheels on revolute joints
├── world/
│   └── apartment.world   # apartment + two my_robot instances + cafe table and cabinet from the Gazebo online library
├── script/
│   └── welcome.cpp       # WorldPlugin that prints the welcome message
└── CMakeLists.txt        # builds script/welcome.cpp into libwelcome.so
```

Build the plugin and launch the world:

```sh
cd /home/workspace/myrobot/build_my_world
mkdir build && cd build
cmake .. && make
export GAZEBO_PLUGIN_PATH=/usr/lib/x86_64-linux-gnu/gazebo-11/plugins:/home/workspace/myrobot/build_my_world/build
cd ../world
gazebo apartment.world
```

`Welcome to Viviane's World!` is printed in the terminal, and Gazebo opens with the apartment, both robots, the table and the cabinet. The first launch downloads the table and cabinet from the online library, so it can take a minute.

Robot dimensions:

| Part    | Shape    | Size                          | Notes                                         |
|---------|----------|-------------------------------|-----------------------------------------------|
| Chassis | Box      | 0.4 x 0.3 x 0.1 m             | 0.1 m above the ground                        |
| Wheels  | Cylinder | radius 0.08 m, length 0.04 m  | roll = 1.5707 rad, revolute joint about the wheel's Z axis |

## Gazebo Basics lesson

### Layout

```
myrobot/
├── model/
│   ├── robot/        # two-wheeled robot: box chassis + two cylinder wheels on revolute joints
│   ├── building/     # U-shaped brick house: 10 m x 10 m, 2.5 m wide wings, one door, two windows
│   └── vehicle/      # Gazebo "Model Editor" tutorial vehicle: chassis, two front wheels, caster, depth camera, follower plugin
├── world/
│   └── myworld       # empty world + the robot + the hello plugin
├── script/
│   └── hello.cpp     # WorldPlugin that prints "Hello World!"
└── CMakeLists.txt    # builds script/hello.cpp into libhello.so
```

### Run it

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

### Add the house

To make the models show up under Gazebo's **Insert** tab, add the model folder to the model path before launching:

```sh
export GAZEBO_MODEL_PATH=${GAZEBO_MODEL_PATH}:/home/workspace/myrobot/model
```

Then pick `building` (or `robot`) from the Insert tab and click in the scene to place it.

### Robot dimensions

| Part    | Shape    | Size                         | Pose                         |
|---------|----------|------------------------------|------------------------------|
| Chassis | Box      | 0.3 x 1.0 x 0.1 m            | z = 0.2                      |
| Wheels  | Cylinder | radius 0.2 m, length 0.1 m   | z = 0.2, roll = 1.5707 rad   |

Each wheel is attached to the chassis with a revolute joint about the wheel's Z axis.
