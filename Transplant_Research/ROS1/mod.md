~~我所简单理解的我们在 `arceOS` 上完成对于 `ROS1` 的支持主要包括对于他们提供的核心库进行分析（比如说分析他们使用的 [rosdep](https://github.com/ros-infrastructure/rosdep), [rosinstall](https://github.com/vcstools/rosinstall) 等工具是如何根据其源代码仓库中的内容生成对应的信息的，然后我们通过自己写的脚本，基于应用层面的需求，拆分各个组件的生成，实现对应的系统调用或者其他的内容）~~

~~应用层面的需求：首先要能支持根据需要，生成只包含单个应用程序的二进制分发，同时还要提供一个附带 `shell` 以及一系列例如 `rosbag` `rostopic`之类的命令行工具，帮助用户在需要进行调试的情况下能够有一个相对比较好的体验。~~

~~至于目前我们正在研究的 [rosrust](https://github.com/adnanademovic/rosrust) 的移植问题，我感觉反倒不是我们现在所面临的主要问题，我简单的查看了下这个仓库的代码，发现其基本上就相当于提供了一个通过 Rust 调用 ROS 框架的接口，不包含任何有关于 ROS1 内部库的实现。这一点上，他与现存的 `roscpp`, `rospy` 等似乎并不存在什么区别。~~



~~可以通过 `rospack` 查看 `catkin` 包的间接依赖~~

（`rosnode` 可以检出 `ros-core-rs` 提供的 `master` 存在，但是没有办法找到对应的 `/rosout` 节点。）



```
RUST_LOG=info ROSRUST_MSG_PATH=`realpath examples/chatter/msgs` cargo run --example chatter --release
```







### ros-core-rs

- 是可以生成一个能被 `ROS1` 检出的 `roscore` 程序，但是没有办法在 `ROS1` 框架中使用 `rosnode list` 等方式检出 `/rosout` 节点（但是是可以正常接受到对应的信息并返回注册成功的）
- 但是 `rostopic echo` 等方法如果尝试打印对应 `topic` 信息的时候会 `panic`
- 测试出来结果是正常的，我们应该可以通过对于 `Master` 节点实现的学习，向上面移植一些简单的 `ROS1` 提供的命令行工具







































