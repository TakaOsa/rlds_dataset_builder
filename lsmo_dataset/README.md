# LSMO Dataset

This dataset contains trajectories collected from Cobotta (Denso wave Inc.) by executing the trajectories planned by using LSMO proposed in [this paper](https://journals.sagepub.com/doi/full/10.1177/02783649211044405) .

## Dataset
The dataset contains trajectories from 50 trajectories, and Cobotta has 6 DoFs.
The first 25 trajectories correspond to the trajectories shown in Figure 34(c) in [the paper](https://journals.sagepub.com/doi/full/10.1177/02783649211044405), and the rest of the trajectories correspond to those in Figure 37(c) in [the paper](https://journals.sagepub.com/doi/full/10.1177/02783649211044405). The dataset contains 25 different ways to reach the goal.

Although the commands to control the robot were sent every 8 ms in the experiments, the trajectories in the dataset were downsampled due to the frame rate of the camera.

## Features

The features in each trajectory are:
1. `image` - Type: `np.uint8`, shape: `(120, 120, 3)`. Main camera RGB observation. Although LSMO does not take images as its input, we recorded the images at every step.

2. `state` - Type: `np.float32`, shape: `(13,)`. The state contains the end-effector position and joint angles. `state[:3]` is the end-effector position (x,y,z), `state[3:6]`  is the Euler angle of the end-effector, `state[6:12]` is the joint angle of the robot, and `state[12]` is the gripper state.

3. `action` - Type: `np.float32`, shape: `(7,)`. The action consists of the deviation of the position and orientation of the end-effector. `action[:3]` is the deviation of the position, `action[3:6]` is the deviation of the Euler angle, and `action[6]` is the gripper state. We assumed that the gripper will open if `action[6]=0`, and the gripper will close if `action[6]=1`. As our dataset contains only reaching motions, `action[6]=0` in all samples.

Please note that the Euler angle of the end-effector was computed using `scipy.spatial.transform.Rotation` from the rotation matrix, which can be obtained by solving the forward kinematics.
The Euler angle was computed using `r.as_euler('zyx', degrees=False)`, and please make sure that the definition of the Euler angle is suitable for your use. 

## License
CC-BY-4.0