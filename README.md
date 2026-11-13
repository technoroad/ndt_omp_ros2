> [!NOTE]
> **This is an unofficial mirror of
> [rsasaki0109/ndt_omp_ros2](https://github.com/rsasaki0109/ndt_omp_ros2)**
> (itself derived from [koide3/ndt_omp](https://github.com/koide3/ndt_omp)).
> It is maintained by technoroad as the submodule source for
> [technoroad/lidar_slam_ros2](https://github.com/technoroad/lidar_slam_ros2).
> It is not affiliated with or endorsed by the upstream authors.
>
> - Snapshot taken on 2026-11-13. Not synced with upstream.
> - khepri_slams uses the tag `technoroad/khepri-pin`
>   (`41bdfba539bb4fe8becd4bc6cf9cd5e5ad35dab1`).
> - Upstream pull request refs are preserved under `refs/upstream-pull/*`.
> - Please report bugs and send contributions to the upstream repository.
> - License: BSD 2-Clause, unchanged from upstream (see `LICENSE`).
>
> 本リポジトリは rsasaki0109/ndt_omp_ros2（元は koide3/ndt_omp）の非公式ミラーです。
> technoroad/lidar_slam_ros2 の submodule の取得元として technoroad が保管しています。
> 不具合報告や貢献は本家へお願いします。

# ndt_omp_ros2
This package provides an OpenMP-boosted Normal Distributions Transform (and GICP) algorithm derived from pcl. The NDT algorithm is modified to be SSE-friendly and multi-threaded. It can run up to 10 times faster than its original version in pcl.

### Benchmark
```
$ cd ~/ros2_ws/src/ndt_omp_ros2/data
$ ros2 run ndt_omp_ros2 align 251370668.pcd 251371071.pcd

--- pcl::GICP ---
single : 267.385[msec]
10times: 1151.76[msec]
fitness: 0.220382

--- pclomp::GICP ---
single : 173.152[msec]
10times: 1299.14[msec]
fitness: 0.220388

--- pcl::NDT ---
single : 425.142[msec]
10times: 3638.77[msec]
fitness: 0.213937

--- pclomp::NDT (KDTREE, 1 threads) ---
single : 308.935[msec]
10times: 3095.53[msec]
fitness: 0.213937

--- pclomp::NDT (DIRECT7, 1 threads) ---
single : 188.942[msec]
10times: 1373.47[msec]
fitness: 0.214205

--- pclomp::NDT (DIRECT1, 1 threads) ---
single : 41.3584[msec]
10times: 347.261[msec]
fitness: 0.208511

--- pclomp::NDT (KDTREE, 8 threads) ---
single : 108.68[msec]
10times: 1046.16[msec]
fitness: 0.213937

--- pclomp::NDT (DIRECT7, 8 threads) ---
single : 56.9189[msec]
10times: 545.279[msec]
fitness: 0.214205

--- pclomp::NDT (DIRECT1, 8 threads) ---
single : 16.7266[msec]
10times: 169.097[msec]
fitness: 0.208511
```

Several methods for neighbor voxel search are implemented. If you select pclomp::KDTREE, results will be completely same as the original pcl::NDT. We recommend to use pclomp::DIRECT7 which is faster and stable. If you need extremely fast registration, choose pclomp::DIRECT1, but it might be a bit unstable.

<img src="data/screenshot.png" height="400pix" /><br>
Red: target, Green: source, Blue: aligned
