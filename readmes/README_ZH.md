
<p align="left">
    中文</a>&nbsp ｜ &nbsp<a href="../README.md">English</a>&nbsp
</p>
<br><br>

在这个项目中，你可以了解到以下内容

* 快速了解关于大模型水印技术的流派以及特点
* 快速上手TuningScope教程，玩转大模型水印
* 获得丰富的中文水印语料库
* 大模型水印相关细节，包括三种不同模式的水印方法
* 水印添加与验证性能数据，包括额外耗时和额外显存占用
* 部署教程，以ChatGLM 和 GPT-3.5-turbo为例
* 搭建API的方法，我们提供的示例为OpenAI风格的API
* 开源水印工作评测
* 特定领域（如代码领域）的水印技术概览
* 使用协议
* ...

如果遇到问题，请优先考虑查询[FAQ](./question/FAQ.md)。如仍未解决，随时提出issue（但建议使用英语或提供翻译，有助于帮助更多用户）。如果想帮助我们提升，欢迎提交Pull Requests！

想和我们一起讨论和聊天的话，赶紧加入我们的微信群和Discord server（入口见文档开头部分）！
<br><br>



# 针对大语言模型的水印框架


## 技术图谱（普通文本）

本节中，我们将会介绍目前社区主流的三种水印添加方法，包括每种方法对应的代表论文以及优劣解析，有关更多论文的指引或解析（以及未来实现的路线图），请参考[技术路线图](../roadmap/overview_ZH.md)

**目前我们将所有收录并预计实现的技术文章分为(1) 正式发表于国际顶级计算机会议. (2) 预印本. (3) 博客（或产品原型）. (4) 其他.**


| 水印方法名   | 实现难度 | 灵活性 | 鲁棒性 | 语义保持 | 水印比特限制 | 时间代价 | 计算资源代价 | 
| ---         | --- | --- | --- | --- | --- | --- | --- | 
| 基于规则水印 | ★       | ★★      | ★★★   | ★★★★   | 无  | 低    | 低  | 
| 推理时水印   | ★★★    | ★★★    | ★★★   | ★★      | 无  | 中    | 高    | 
| 神经网络水印 | ★★★    | ★★★★   | ★★★★ | ★★★     | 有  | 高    | 中 | 

以下为具体的技术概览图，三张图片版权归原论文作者所有

<img src=../fig/Overview.png width=100% />



### 基于规则的水印

本节主要以 Monash 大学 Xuanli He 的工作[1,2] 为样本，该系列工作已发表在 AAAI, NIPS上， [论文实现在此](https://github.com/xlhex/NLG_api_watermark) 

基于规则的水印技术是一种文本水印技术，它通过替换同义词或转换段落中的句法结构来插入水印。人工设计的特征通过词的分布或句法分析，使插入的签名在统计上可被删除。这种方法因其简单易行而被广泛用于文本水印。基于规则的方法可确保插入的水印在统计学上对攻击具有鲁棒性，使攻击者很难在不显著改变文本含义的情况下去除水印。这种技术在生成的文本预计会经历多次转换的情况下特别有用，例如机器翻译或文本摘要。
基于规则的水印方法有几个优点。它易于实现，可应用于任何文本生成模型。此外，插入的水印在统计学上是稳健的，因此很难在不显著改变文本含义的情况下将其移除。不过，这种方法也有一些局限性。插入的水印在语义上与生成的文本不一致，因此很容易被检测到。此外，水印签名长度受限于文本中可替换的同义词或句法结构的数量，阻碍了其实际应用。


[1] He, Xuanli, et al. "Protecting intellectual property of language generation apis with lexical watermark." Proceedings of the AAAI Conference on Artificial Intelligence. Vol. 36. No. 10. 2022. **AAAI 2022**

[2] He, Xuanli, et al. "Cater: Intellectual property protection on text generation apis via conditional watermarks." Advances in Neural Information Processing Systems 35 (2022): 5431-5445. **NIPS 2023**

### 推理阶段水印

本节主要以 Maryland大学 John Kirchenbauer的工作[3] 为样本， 该工作已发表在 ICML 2023上，[论文实现在此](https://github.com/jwkirchenbauer/lm-watermarking)。

推理阶段水印技术是另一种文本水印技术，其核心在于在LLM decoding 阶段对于生成的文本进行限制。

它将词汇分成绿/红列表，并限制 LLM 解码从绿列表中预测下一个标记。这种技术可确保插入的水印在统计学上对攻击具有鲁棒性，使攻击者很难在不显著改变文本含义的情况下去除水印。不过，这种解码策略会极大地扭曲加了水印的 LLM 输出与原始 LLM 输出之间的语义相似性。
推理阶段水印有几个优点。插入的水印在统计上是稳健的，因此很难在不显著改变文本含义的情况下将其去除。此外，这种技术可应用于任何包含decoding阶段的文本生成模型。不过，解码策略会极大地扭曲加了水印的 LLM 输出与原始 LLM 输出之间的语义相似性，因此很容易被检测到。此外，这种方法在语义上与生成的文本不一致，妨碍了它的实际应用。



[3] Kirchenbauer, John, et al. "A watermark for large language models." arXiv preprint arXiv:2301.10226 (2023). **ICML 2023**




### 基于神经网络的水印技术


本节主要以 CISPA 研究所 Sahar Abdelnabi的工作[4] 为样本， 该工作已发表在 S&P 21上，[论文实现在此](https://github.com/S-Abdelnabi/awt)。



基于神经网络的水印技术是一种文本水印技术，它利用端到端学习技术将二进制水印签名整合到 LLM 生成的文本中，同时保持语义的一致性。这种方法可确保插入的水印在统计上具有抗攻击性，同时与生成的文本保持语义一致。然而，与基于规则的框架和推理时间框架相比，每个标记段的最大可编码签名长度有限，阻碍了这种方法的实际应用。

基于神经网络的水印技术有几个优点。插入的水印在统计上是稳健的，因此很难在不显著改变文本含义的情况下将其移除。此外，这种方法与生成的文本在语义上保持一致，因此很难被检测到。不过，每个标记段的最大可编码签名长度有限，妨碍了它的实际应用。

[4] Abdelnabi, Sahar, and Mario Fritz. "Adversarial watermarking transformer: Towards tracing text provenance with data hiding." 2021 IEEE Symposium on Security and Privacy (SP). IEEE, 2021.

## 技术图谱（其他）

本节中，我们将会介绍目前**正式发表于国际顶级计算机会议**的针对不同于对话文本的水印添加方法，包括每种方法对应的代表论文以及优劣解析，有关更多论文的指引或解析（以及未来实现的路线图），请参考[额外技术路线图](../roadmap/additional_overview_ZH.md)




| 论文        | 研究机构   | 针对对象 | 技术路线 | 语义保持  | 时间代价 | 计算资源代价 | 
| ---         | --- | --- | --- | --- | --- | --- | 
| SSLGuard[1]   | CISPA     | Encoders | Retrain      | -   | 中   |  高  | 
| CodeWatermark[2] | HKUST | Code | Rule-based    | ★★★★  | 低   |  低  | 


[1] Cong, Tianshuo, Xinlei He, and Yang Zhang. "SSLGuard: A watermarking scheme for self-supervised learning pre-trained encoders." **CCS 2022**

[2] Zongjie Li, et al. "Protecting Intellectual Property of Large Language Model-Based Code Generation APIs via Watermarks." **CCS 2023**




## 要求

* python 3.10及以上版本
* pytorch 2.0.1及以上版本，推荐2.1及以上版本
* 建议使用CUDA 12.2及以上（GPU用户需考虑此选项，否则部分水印方法无法执行或执行速度过慢）
<br>

## 快速使用

我们提供简单的示例来说明如何利用🤖 TuningScope 和🤗 Transformers快速使用TuningWM。

在开始前，请确保你已经配置好环境并安装好相关的代码包。最重要的是，确保你满足上述要求，然后安装相关的依赖库。

我们针对位于网络访问特殊地段的用户特殊配置了相关的包安装指南，以方便用户使用（此内容不存在于英文readme中）



## 引用
如果你觉得我们的工作对你有帮助，欢迎引用！

```
@article{
}
```
<br>

## 使用协议

研究人员可以使用 TuningScope 框架进行研究活动。除特殊说明（例如引自第三方内容），仓库中关于不同技术解析的内容版权归项目组所有，如需使用，请填写问卷[解析内容]()进行申请。

我们同样允许商业使用，具体细节请查看[LICENSE](LICENSE)。如需商用，请填写问卷[商业应用]()申请。
<br><br>

## 联系我们

如果你想给我们的研发团队和产品团队留言，欢迎加入我们的微信群和Discord server。当然也可以通过邮件（qianwen_opensource@alibabacloud.com）联系我们。