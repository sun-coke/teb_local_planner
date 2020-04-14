# teb_local_planner包配置坑点笔记
## 1、根据CMakefile配置teb依赖包
![image](https://github.com/sun-coke/teb_local_planner/blob/master/1.png)

## 2、安装g2o优化器

××git源码：××
```
git clone https://github.com/RainerKuemmerle/g2o.git
```

××依赖更新：××
```
sudo apt-get install libeigen3-dev 
sudo apt-get install libsuitesparse-dev 
sudo apt-get install libqt5-dev 
sudo apt-get install qt5-qmake 
sudo apt-get install libqglviewer-dev
```

××编译：××
```
cd g2o
mkdir build
cd build
cmake ..
make
```

××安装：××
```
sudo make install
```
方法二（未尝试）
```
sudo apt-get install libpcl-dev pcl-tools
```

## 3、配置稀疏矩阵库
```
$ apt-cache search suitesparse # 查找apt源
$ sudo apt-get install libsuitesparse-dev‘
$ sudo apt-get install libboost-all-dev
```
## 4、git下载teb包源代码并编译

```
cd catkin_ws/src
git clone https://github.com/sun-coke/teb_local_planner.git
cd catkin_ws && catkin_make
```

## 5、解决catkin_make编译出错
错误提醒：
![image](https://github.com/sun-coke/teb_local_planner/blob/master/2.png)


修改teb_local_planner/src/optimal_planner.cpp中g2o初始化函数第159、160行指针声明方式“std::unique_ptr”
```
  TEBBlockSolver* blockSolver = new TEBBlockSolver(std::unique_ptr<TEBLinearSolver>(linearSolver));
  g2o::OptimizationAlgorithmLevenberg* solver = new g2o::OptimizationAlgorithmLevenberg(std::unique_ptr<TEBBlockSolver>(blockSolver));

```

## 6、编译集成
重新编译即通过（注：代码修改后需要重新编译生效）
通过插件将teb集成到move_base中，参考move_base笔记


