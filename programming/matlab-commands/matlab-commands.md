# MatLab 常用命令

首次提交：2017-12-11 by [🐟](https://github.com/tyusr)

## 代码编写

* 文档查询
    ```matlab
    % fun 为待查询的函数，在命令行输入
    doc fun
    % 或：
    help fun
    ```

* 清除命令行窗口、工作区、关闭其他窗口。这三个命令经常用在脚本的开始
    ```matlab
    clc % 命令行窗口清屏
    clear % 清除工作区变量
    close all % 关闭所有 figure
    ```

## 调试

* 运行计时
    ```matlab
    tic;
    % ... 待计时的代码段
    t = toc; % t 即为所耗时间（秒）

    % 或者，所耗时间将直接输出在命令行窗口
    tic
    % ...
    toc
    ```

* 代码运行出错时，自动在出错的那一行设置断点并停止
    ```matlab
    % 在命令行输入
    dbstop if error
    ```

## 其他工具

* 颜色阈值查看（包含 RGB, HSV, YCbCr, Lab 等颜色空间）
    ```matlab
    % 在命令行输入
    colorThresholder
    ```