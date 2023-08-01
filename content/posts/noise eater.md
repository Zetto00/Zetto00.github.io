---
title: "noise eater 使用"
date: 2023-07-14T13:48:00+08:00
draft: false
toc: true
description: "项目笔记"
tags: [项目笔记]
categories: [科研]
---
<!--more-->

[液晶噪声衰减器/激光振幅稳定器 | thorlabs](https://www.thorlabs.com/newgrouppage9.cfm?objectgroup_ID=6107)

# 调节方式
调整 noise eater 前的半波片，使得 noise eater 后功率最小。

将档位旋至合适，旋钮旋至最大，逐渐旋小旋钮，当 noise eater 上灯从红变绿，调节完成。

旋钮越旋小，出射功率越低。一般低了半波片旋至最大且 noise eater 不启动时的功率的 10%为佳。

# 原理
{{< image src="https://www.thorlabs.com/images/TabImages/Noise_Eater_Schematic_D2-780.gif" caption="Noise Eater Schematic">}}

{{< image src="/images/科研/noise%20eater_2023-07-10_19.57.14.excalidraw.png" caption="原理示意图">}}

## 内部PID算法
不同档位设定了不同目标电压，旋钮旋转改变目标电压。PID 算法保证信号电压锁至目标电压。

过小的档位会导致过小的最大出光功率。过大的档位会导致亮红灯，锁不到目标电压。

## 前置半波片调最小原理
### 假说一
液晶波片需要启动电压才能使排列从杂乱无序到整齐划一。调节前置波片使出光功率最小，即使是最低档位（1mW）的目标电压也高于初始电压，这个差值使得 PID 施加在液晶波片上的电压高于启动电压。

### 假说二
初始电压信号过大会使得 PID 算法根本不起作用。
- TODO 验证办法：半波片调节至一半，观察档位和功率关系

# beam splitter 不同分光比影响
测得原始分光比为 85:15。

若分给功率探头的光在相同入射光功率下比原始分光比的多，即上图曲线幅值增大，那么档位相对调高。