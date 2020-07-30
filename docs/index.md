# Tasks

The Hard-Eight task suite contains the following tasks:

*   `use_object_knock_down`: referred as **Baseball** in the paper.
*   `drawbridge_max3fork_safe_randomwall`: referred as **Drawbridge** in the
    paper.
*   `ball_room_navigation_cubes`: referred as **Navigation Cubes** in the paper.
*   `slot_colored_block`: referred as **Push Blocks** in the paper.
*   `sensorhidden_color_novary_greywall`: referred as **Remember Sensor** in the
    paper.
*   `throw_across`: referred as **Throw Across** in the paper.
*   `reach_wall`: referred as **Wall Sensor** in the paper.
*   `reach_wall_require_stack`: referred as **Wall Sensor Stack** in the paper.

For a more complete description of the tasks, consult the accompanying
[R2D3 paper](https://arxiv.org/abs/1909.01387).

You can find videos of the tasks at
<https://deepmind.com/research/publications/r2d3>.

# Actions

The environment provides the following actions:

*   `MOVE_BACK_FORWARD`: range `[-1.0, 1.0]`
*   `STRAFE_LEFT_RIGHT`: range `[-1.0, 1.0]`
*   `LOOK_DOWN_UP`: range `[-1.0, 1.0]`
*   `LOOK_LEFT_RIGHT`: range `[-1.0, 1.0]`
*   `HAND_ROTATE_AROUND_FORWARD`: range `[-1.0, 1.0]`
*   `HAND_ROTATE_AROUND_RIGHT`: range `[-1.0, 1.0]`
*   `HAND_ROTATE_AROUND_UP`: range `[-1.0, 1.0]`
*   `HAND_PUSH_PULL`: range `[-10.0, 10.0]`
*   `HAND_GRIP`: range `[0.0, 1.0]`

Each action is a `double` scalar. It is not compulsory to send a value for each
action every step, but note that actions are "sticky", meaning an action's value
will only change when a new value is provided. For example:

```python
env = dm_hard_eight.load_from_docker(settings)
env.reset()
env.step({'STRAFE_LEFT_RIGHT': -1.0}) # Result: strafe Left.
env.step({'MOVE_BACK_FORWARD': 1.0}) # Result: strafe left & move backward.

env.step({'STRAFE_LEFT_RIGHT': 0.0,
          'MOVE_BACK_FORWARD': 0.0}) # Result: stationary.
```

# Observations

The environment provides the following observations:

*   `RGB_INTERLEAVED`: First person RGB camera observation. The `width` and
    `height` can be adjusted through the `EnvironmentSettings`, but the
    observation will always have a fixed 4:3 aspect ratio.
*   `ACCELERATION`: The avatar's "acceleration" vector, in meters/second^2. The
    components are measured, in the (right, up, forward) directions relative to
    the direction the avatar is facing. This is represented as a vector of 3
    floats.
*   `HAND_FORCE`: The force, as a fraction of the maximum grip strength, the
    hand is applying to the object it's manipulating. If holding an object, this
    represents how close to dropping it the avatar is. This is represented as a
    float with a value between 0 and 1.
*   `HAND_IS_HOLDING`: Set to 1 if the avatar is holding an object, 0 otherwise.
*   `HAND_DISTANCE`: Distance of hand from face, expressed as a fraction of min
    and max distance. 0 when not holding an object, otherwise between 0 and 1.
*   `Score`: The agent's cumulative score.

# Configurable environment settings

Required attributes:

*   `seed`: Seed to initialize the environment's random number generator.
*   `level_name`: Name of the level to load as they appear in the first section.

Optional attributes:

*   `width`: Width (in pixels) of the desired RGB observation; defaults to 96.
*   `height`: Height (in pixels) of the desired RGB observation; defaults to 72.
*   `num_action_repeats`: Number of times to step the environment with the
    provided action in calls to `step()`.
