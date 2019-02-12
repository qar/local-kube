# Local-Kube

> 在本地搭建 Kubernetes 集群的方案

## 依赖

[vagrant](https://www.vagrantup.com/)

## 如何使用

`vagrant up` 虚拟机，第一次启动时会通过 Vagrantfile 中 `provision` 设置中的命令安装 Kubernetes。安装完毕后通过 `vagrant ssh` 进入虚拟机操作。
