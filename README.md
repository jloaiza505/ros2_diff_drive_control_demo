# ROS 2 Control Demo Workspace

This repository is a ROS 2 workspace for a simple differential-drive robot using:
- `my_robot_description` for URDF/Xacro, RViz config, and display launch files
- `my_robot_bringup` for ros2_control controllers and full bringup launch

## Workspace Structure

```
ros2_control_demo/
├── src/
│   ├── my_robot_description/
│   │   ├── urdf/
│   │   ├── launch/
│   │   └── rviz/
│   └── my_robot_bringup/
│       ├── config/
│       └── launch/
├── build/      # generated
├── install/    # generated
└── log/        # generated
```

## Prerequisites

- Ubuntu with ROS 2 Humble installed
- `colcon` available
- ROS 2 packages used by this workspace:
  - `xacro`
  - `robot_state_publisher`
  - `joint_state_publisher_gui`
  - `rviz2`
  - `controller_manager`
  - `ros2_control`
  - `ros2_controllers`

## Build

Run from the workspace root:

```bash
source /opt/ros/humble/setup.bash
colcon build
source install/setup.bash
```

## Run

### 1. Visualize Description Only

```bash
source /opt/ros/humble/setup.bash
source install/setup.bash
ros2 launch my_robot_description display.launch.xml
```

### 2. Full Bringup (ros2_control + RViz2)

```bash
source /opt/ros/humble/setup.bash
source install/setup.bash
ros2 launch my_robot_bringup my_robot.launch.xml
```

This launch starts:
- `robot_state_publisher`
- `ros2_control_node`
- `joint_state_broadcaster`
- `diff_drive_controller`
- `rviz2`

## Useful Checks

```bash
ros2 control list_controllers
ros2 topic echo /joint_states
ros2 topic list | grep diff_drive
```

## Key Files

- Robot description entrypoint: `src/my_robot_description/urdf/my_robot.urdf.xacro`
- Base model and wheel joints: `src/my_robot_description/urdf/mobile_base.xacro`
- ros2_control block: `src/my_robot_description/urdf/mobile_base.ros2_control.xacro`
- Controller config: `src/my_robot_bringup/config/my_robot_controllers.yaml`
- Bringup launch: `src/my_robot_bringup/launch/my_robot.launch.xml`

## Troubleshooting

- RViz2 TF errors like `No transform from [right_wheel_link] to [base_footprint]`:
  - Ensure `ros2_control_node` receives the same `robot_description` as `robot_state_publisher`.
  - Ensure `joint_state_broadcaster` is active and `/joint_states` contains wheel joints.

- Launch file changes not reflected:
  - Rebuild and re-source:
    ```bash
    colcon build
    source install/setup.bash
    ```

## Notes

- `.gitignore` excludes generated ROS 2 workspace artifacts (`build/`, `install/`, `log/`) and editor/system files.
- `package.xml` files still contain placeholder description/license fields; update them before publishing.

