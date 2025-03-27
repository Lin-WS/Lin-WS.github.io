---
title: MATLAB计算功率谱密度
tags:
  - MATLAB
categories:
  - 信号处理
  - 功率谱密度
toc: true
sticky: false
date: 2025-03-27 11:48:06
---


MATLAB计算pulse数据的功率谱密度

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
channel1 = 1; % 通道
[PSD, freq] = pwelch(data(:, channel1), window, overlap, nfft, FS); % 功率谱密度
%% 作图
figure; 
plot(freq, 10 * log10(PSD), 'LineWidth', 2);
hold on;
xlabel('频率 / Hz', 'FontSize', 15, 'FontName', 'Microsoft YaHei'); % 横轴标题
ylabel('功率谱密度 / dB', 'FontSize', 15, 'FontName', 'Microsoft YaHei'); % 纵轴标题
set(gca, 'FontSize', 13); % 设置整个坐标轴的字体大小
title('功率谱密度', 'FontSize', 15, 'FontName', 'Microsoft YaHei'); % 设置标题
set(gca, 'XScale', 'log'); % 对数刻度

```