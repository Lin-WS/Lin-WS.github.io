---
title: MATLAB读取Excel数据
categories:
  - 信号处理
  - Excel
toc: true
sticky: false
date: 2025-03-27 11:37:58
tags:
---


matlab读取excel文件数据，适用于测试时手动记录数据的场景，一般情况是频率+工况+数据


<!--more-->

```matlab
clc;
clear;
close all;

%% 读取Excel文件数据并作图
% 指定Excel文件路径
filePath = ""; % 替换为Excel文件路径
% 读取Excel文件的几列数据
data = readmatrix(filePath, 'Sheet', 3); % 指定读取Excel文件的第 3 个工作表
column1 = data(:, 1); % 第 1 列数据
column2 = data(:, 6); % 第 6 列数据

% 作图
% figure;
hold on;
plot(column1, column2, '-o', 'LineWidth', 2);
% xlabel('Column 1', 'FontSize', 13, 'FontName', 'Microsoft YaHei');
% ylabel('Column 2', 'FontSize', 13, 'FontName', 'Microsoft YaHei');
% title('Excel Data Plot', 'FontSize', 13, 'FontName', 'Microsoft YaHei');
```