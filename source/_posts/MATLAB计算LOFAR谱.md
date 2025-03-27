---
layout: matlab计算加速度灵敏度
title: MATLAB计算LOFAR谱
tags:
  - MATLAB
categories:
  - 信号处理
  - LOFAR谱
toc: true
sticky: false
date: 2025-03-27 11:48:44
---


MATLAB计算pulse数据的LOFAR谱

<!--more-->

```MATLAB
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
figure;
[S, f, t] = spectrogram(data(:, channel1), window, overlap, nfft, FS);
axis xy;
SdB = 20 * log10(abs(S)/(max(max(abs(S)))));
SdB(SdB <= -80) = -79.99;
imagesc(t, f, SdB);
set(gca, 'YDir', 'normal');
colormap('jet');
colorbar;
title(['lofar谱 - ', '通道', num2str(j)], 'Interpreter', 'none', 'FontSize', 13, 'FontName', 'Microsoft YaHei');
xlabel('time /s', 'FontSize', 13, 'FontName', 'Microsoft YaHei');
ylabel('freq /Hz', 'FontSize', 13, 'FontName', 'Microsoft YaHei');
        
```