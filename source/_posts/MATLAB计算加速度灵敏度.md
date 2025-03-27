---
title: MATLAB计算加速度灵敏度
tags:
  - MATLAB
categories:
  - 信号处理
  - 加速度灵敏度
toc: true
sticky: false
date: 2025-03-27 11:51:55
---


利用互谱法计算振动传感器的加速度灵敏度

<!--more-->

```matlab
clc; 
clear;
% close all;
fileName = "";  % 数据文件
[data, para] = read_dat_file(fileName);
FS = para.SampleFrequency; % 采样率
nfft = 2 ^ nextpow2(FS); % FFT点数（分段处理）
window = hann(nfft); % 汉宁窗
overlap = 0.5 * nfft; % 50重叠
channel_ref = 1; % 参考加速度计通道
channel_test = 2; % 待测加速度计通道
gain_ref = 1; % 参考加速度计测试增益，单位 (倍)
gain_test = 1; % 待测加速度计增益，单位 (倍)
M_ref = 1; % 参考加速度计电压灵敏度，单位 (V/ms-2)
figure;
nfft = 2 ^ nextpow2(FS(i)); % FFT点数（分段处理）
% 计算参考加速度计的自谱
[PSD, ~] = pwelch(data(:, channel_ref), window, overlap, nfft, FS);
% 计算待测和参考的互谱
[CPSD, freq] = cpsd(data(:, channel_test), data(:, channel_ref), window, overlap, nfft, FS);
% 计算加速度灵敏度，单位 (mV/ms-2)
M_test = M_ref * gain_ref / gain_test * abs(CPSD ./ PSD) * 1000;
plot(freq, M_test, 'LineWidth', 2);
hold on;
set(gca, 'XScale', 'log'); % 对数刻度
% title('加速度灵敏度', 'FontSize', 13, 'FontName', 'Microsoft YaHei');
ylabel('加速度灵敏度 /(mV/ms-2)', 'FontSize', 13, 'FontName', 'Microsoft YaHei');
xlabel('freq /Hz', 'FontSize', 13, 'FontName', 'Microsoft YaHei');
```