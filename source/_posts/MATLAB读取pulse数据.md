---
title: MATLAB读取pulse数据
tags:
  - MATLAB
categories:
  - 信号处理
  - pulse
toc: true
sticky: false
date: 2025-03-27 13:56:46
---


MATLAB读取B&K公司pulse测量系统数据的程序

<!--more-->

```matlab
% @para filename pulse采集的数据文件，一般为".dat"后缀
% @return data 原始数据，一个矩阵，每一列表示一个通道的数据
% @return para 结构体，记录数据时的基本设置，包括采样率，数据类型等等
function [data, para] = read_dat_file(filename)
    fid = fopen(filename, 'rb');
    para.device = fgetl(fid);
    tline = fgetl(fid);
    para.SampleFrequency = fscanf(fid, 'SampleFrequency=%d\n');
    NoChannels = fscanf(fid, 'NoChannels=%d\n');
    para.DataType = fgetl(fid);
    para.DataBitSize = fscanf(fid, 'DataBitSize=%d\n');
    para.FormatID = fgetl(fid);
    para.Version = fgetl(fid);
    
    CorrectionFactor = ones(1, NoChannels);
    Offset = zeros(1, NoChannels);
    OverloadRatio = zeros(1, NoChannels);
    for ichannel = 1:NoChannels
        tline = fgetl(fid);
        tempvalue = fscanf(fid, 'CorrectionFactor=%f');
        tline = fgetl(fid);
        CorrectionFactor(ichannel) = tempvalue;
        tempvalue = fscanf(fid, 'Offset=%f');
        tline = fgetl(fid);
        Offset(ichannel) = tempvalue;
        tempvalue = fscanf(fid, 'OverloadRatio=%f');
        tline = fgetl(fid);
        OverloadRatio(ichannel) = tempvalue;
    end
    
    tline = fgetl(fid);
    startpoint = ftell(fid) + 20;
    fseek(fid, 0, 'eof');
    position = ftell(fid);
    block = (position - startpoint) / 8208 / NoChannels;
    
    fseek(fid, startpoint, 'bof');
    len = block * 2048;
    data = zeros(len, NoChannels);
    
    for iblock = 1:block
        for ichannel = 1:NoChannels
            tempc = fread(fid, 4, 'int32');
            index = (iblock - 1) * 2048 + (1:2048);
            data(index, ichannel) = fread(fid, 2048, 'int32');
        end
    end
    
    fclose(fid);
    
    for ichannel = 1:NoChannels
        data(:, ichannel) = data(:, ichannel) * CorrectionFactor(ichannel);
    end
    data = data * (1/2^16);
end

```