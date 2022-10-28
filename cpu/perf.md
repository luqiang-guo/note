# Perf



## ARM a55

[Arm Cortex-A55 Software Optimization Guide](https://developer.arm.com/documentation/EPM128372/0300/?lang=en)



##  安装

- 1 查看xavier 版本 `head -n 1 /etc/nv_tegra_release`

  L4T 版本R32.4.3

  ```shell
  head -n 1 /etc/nv_tegra_release
  # R32 (release), REVISION: 4.3, GCID: 21589087, BOARD: t186ref, EABI: aarch64, DATE: Fri Jun 26 04:34:27 UTC 2020
  ```

  

- **2** 下载 kernel  [版本链接](https://developer.nvidia.com/embedded/jetson-linux-archive)  选择对应版本[R32.4.3](https://developer.nvidia.com/embedded/linux-tegra-r32.4.3)  [下载源码](https://developer.nvidia.com/embedded/L4T/r32_Release_v4.3/Sources/T186/public_sources.tbz2)。 进入linux 源码 `kernel-4.9/tools/perf`目录。

- **3**  安装依赖`sudo apt install -y libperl-dev libdwarf-dev  systemtap-sdt-dev  `    。编译源码的时候会提示缺少的依赖按照要求安装即可， `get_cpuid` 未解决

  <img src="perf.assets/image-20221027162313919.png" alt="image-20221027162313919" style="zoom:50%;" />

