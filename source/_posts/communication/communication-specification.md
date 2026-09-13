---
title: 无线通信标准杂谈
date: 2022-06-14 16:24:03
tags:
  - 4G LTE
  - 5G NR
  - OFDM
  - 毫米波通信
categories:
  - Communication and Networks
---



## 4G LTE

采用OFDM调制，支持的带宽有（大带宽）10MHz、15MHz、20MHz，（窄带）1.4MHz、3MHz、5MHz。正常情况下子载波间隔15KHz，符号周期66.7us，加上循环前缀长度为71.35us。LTE在时域上的最小单位为时隙，以0.5ms为周期，在15KHz下，0.5ms*15KHz=7.5个正弦波，因此一个时隙内可以容纳7个symbol，多余出来的时间分配给各个符号的循环前缀[^3]。

各个子载波可以根据信道状况采用不同的调制方式，如BPSK，QPSK，8PSK，16QAM，64QAM等；当信道条件好时采用高阶调制方式，而当信道条件差时采用 抗干扰能力强的低阶调制方式[^2]。



## 5G NR

在5G NR中，定义了两种频段范围，FR1和FR2，FR1表示低频频段，FR2表示毫米波高频频段。具体FR1和FR2表示的频段范围如下表所示：

| Frequency range | Frequency range     |
| --------------- | ------------------- |
| FR1 (Sub-6GHz)  | 410MHz - 7125MHz    |
| FR2 (mmWave)    | 24250MHz - 52600MHz |

其中Sub6G频段以3.5GHz为主（3.2GHz-4.2GHz），最大带宽为100MHz。mmWave频段以28/39/60/73GHz为主，带宽50MHz - 400MHz[^6]。

5G仍旧采用OFDM调制，但是不同于4G LTE在于设置了多个不同的子载波间隔，也即$\Delta f=2^\mu \cdot 15$[kHz]，其中$\mu=0,1,2,3,4$，也就是子载波间隔$\Delta f$取值为15/30/60/120/240kHz。

在5G和4G中，帧的长度都是10ms，子帧长度都是1ms。在LTE中，一个子帧包含了14个符号。5G中，当调制符号时间变短时，一个子帧长度1ms内也就有了更多的符号[^4]。与4G不同，5G中的时隙长度不是固定的，每个时隙内都有14个符号，但是随着子载波间隔的不同，对应的时隙不再是固定的0.5ms[^5]。

这些参数看起来很多很复杂，实际上只需要关注两件事。第一个是符号周期，这是由子载波间隔决定的；第二个是帧长度，这个是由5G NR标准规定的固定长度10ms。前者是最小单位，后者是最大单位。至于中间的时隙长度、符号个数等参数，就是在一个帧内对这么多个符号进行组合，至于怎么组合全看协议规定。

在OFDM接收端需要进行时域采样并FFT，这里涉及到两个参数：采样频率和FFT点数。采样频率$f_s$由带宽$B$决定，FFT点数$N=2B\ast T_{\text{symbol}}=2B/\Delta f$，因此FFT点数大约是子载波个数的2倍，和子载波间隔大小无直接关系。尽管如此，子载波间隔越大，符号周期越短，这就要求FFT在更短的时间内完成，并且同等子载波个数情况下信号带宽越大，采样频率越高，对处理器的速度要求越高。

4G以子帧为单位进行调度，而5G则以时隙为单位进行调度。

资源块（RB, Resource block）

还没太搞懂多用户的时频资源是怎么分配的，网上大多语焉不详。



## 5G 毫米波通信

看到一篇博客（2019）[^1]，讲的是5G mmWave的物理层信号PHYs（physical layers）设计。主要观点是现有的物理层信号基本都是基于OFDM，但是当载频高于某个点之后，OFDM就不适合了，需要研究新的物理层信号。3GPP组织目前推出了Release 15和16，频点都低于52.6GHz，在此频段下仍旧采用OFDM波形，但是超过某个频点（也许52.6GHz并不是确切的阈值点）之后，由于实际的物理器件难以实现OFDM信号所需的峰均比、增益平坦度和效率等，在更高频点上不得不设计新的物理信号波形。Nokia、NI等公司在73GHz频点上研究了2GHz带宽的单载波信号波形，在2信道MIMO系统中演示了14.6Gbps的吞吐量。另外802.11ad就是一个单载波调制的毫米波通信协议。





## Reference

[^1]: https://www.edn.com/mmwave-and-ofdm-is-phy-research-dead/
[^2]: https://blog.csdn.net/jyqxerxes/article/details/78981109
[^3]: https://blog.csdn.net/Angel_YJ/article/details/89001077
[^4]: https://blog.csdn.net/u010202588/article/details/94360185
[^5]: https://blog.csdn.net/weixin_39702335/article/details/110309294
[^6]: [5G - Wikipedia](https://en.wikipedia.org/wiki/5G)
