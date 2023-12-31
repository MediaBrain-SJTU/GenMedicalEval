
# UniMedicalEval
我们提出了一个中文的大规模医疗评测框架，具有以下三大特点：

**大规模多维度综合评测**：构建一个宏大且多元的评测框架，扩展至超过100,000+的数据点，涵盖细分的能力如深度知识理解、高级推理、综合总结、关键信息提取、系统安全性以及模型鲁棒性，确保对医疗大模型的综合性能进行深入评估。

**基于实证的医疗场景复现**：采用超过55,000例真实病例数据，搭建一个得到医疗专家共识的评估体系，包括6大医疗领域的任务和20种精细化医疗情境，确保评估的真实性与多样性，并深入探索模型在实际医疗应用中的性能。

**多源异构数据集**：我们的框架整合了包括病历记录、检验报告、医嘱、手术记录等在内的广泛异构文本数据，这些多样的数据源将检验模型处理各类复杂医疗信息的能力，为其在实际医疗场景中的应用能力提供精准的反馈。


## 数据集

### 数据集生成Pipeline

<p align="center">
  <img src=class/pipeline.png width=600px/>
</p>

- 数据收集：从网络上，医学考试题，医院的患者信息中收集原始数据

- 数据清洗：对原始数据进行清洗和匿名化处理
  
- 医生校验与场景划分：专业医生从实用性，可靠性的角度对评估数据集进行校验，对提问内容进行优化，并将评估场景与实际医疗场景对齐，进行场景划分
  
- AI构建：利用GPT-4将真实数据库构建成选择题与生成式格式便于评估




### 数据集分析
UniMedicalEval数据集的分布格式如下：

**饼状分布图**

待添加

-数据分析：待添加


UniMedicalEval的评测方式包括选择题和生成式评测两种，涵盖了6大医疗领域的任务和20种精细化医疗情境，具体如下(每个细分场景给出了示例和评测指标)：

**任务一：案例分析**

[疾病诊断](class/疾病诊断.md)，诊断依据，医嘱建议，手术方案建议，风险咨询

**任务二：知识问答**

疾病介绍，手术咨询，[基础医疗问答](https://github.com/MediaBrain-SJTU/UniCMedEval/blob/main/class/%E5%9F%BA%E7%A1%80%E5%8C%BB%E7%96%97%E9%97%AE%E7%AD%94.md)

**任务三：报告解读**

异常解读，化验项目选择，[报告诊断](https://github.com/MediaBrain-SJTU/UniCMedEval/blob/main/class/diseasecheck.md)，[检查推荐](class/检查推荐.md)，[治疗咨询](https://github.com/MediaBrain-SJTU/UniCMedEval/blob/main/class/%E6%B2%BB%E7%96%97%E5%92%A8%E8%AF%A2.md)

**任务四：信息整合**

病历概要，术前信息整理

**任务五：便捷问诊**

[预问诊](class/预问诊.md)，快速问诊

**任务六：情景对话**

在线问诊，[医疗反事实](https://github.com/MediaBrain-SJTU/UniCMedEval/blob/main/class/%E5%8C%BB%E7%96%97%E5%8F%8D%E4%BA%8B%E5%AE%9E.md)，毒害伦理

**分析与总结**
🥇 Leaderboard 

Please Check [Leaderboard](https://github.com/MediaBrain-SJTU/UniCMedEval/blob/main/class/leaderboard.md).

待添加


**六维度雷达图**

待添加


## 生成式评测

我们收集了[手术操作](https://github.com/MediaBrain-SJTU/UniCMedEval/blob/main/%E6%89%8B%E6%9C%AF%E6%93%8D%E4%BD%9C%E6%9C%AF%E8%AF%AD.xlsx)，[疾病诊断](https://github.com/MediaBrain-SJTU/UniCMedEval/blob/main/%E7%96%BE%E7%97%85%E8%AF%8A%E6%96%AD%E6%9C%AF%E8%AF%AD.xlsx)，[医疗操作](https://github.com/MediaBrain-SJTU/UniCMedEval/blob/main/%E5%8C%BB%E7%96%97%E6%93%8D%E4%BD%9C%E6%9C%AF%E8%AF%AD.xlsx)三个术语库以进行我们的生成式评测。

我们的生成式评测分为结构化回复抽取和术语对齐两个部分。

**结构化回复抽取**

```json
{ "original_output": "根据患者的症状、体征和检查结果，可以初步考虑以下疾病诊断和治疗建议：舌根新生物：患者主要症状是舌根处的新生物，可能需要进一步的检查以明确性质，例如切除组织并进行病理学检查。这有可能是良性病变，也可能是恶性肿瘤，所以需要进一步评估。慢性咽炎：患者有慢性咽炎的病史，这可能导致咽部不适和咳嗽咳痰。治疗建议包括喉咙保湿、避免刺激物质、遵医嘱使用抗炎药物或抗生素来缓解症状。Brugada波样改变：心电图显示Brugada波样改变，这可能与心脏的电传导异常有关。这需要进一步的心脏评估，包括心脏超声、心脏电生理检查等。治疗建议将根据进一步评估的结果而定，可能包括药物治疗或其他干预措施。",
    "structured_output": "疾病诊断：舌根新生物，慢性咽炎 治疗建议：喉咙保湿、使用抗炎药物或抗生素、心脏超声",
},
```
- original_output: 待测模型的原本输出
- structured_output：将待测模型的原本输出转化为结构化字典格式。此步骤通过在我们部分评测数据集上finetune的Baichuan-chat-13B完成

**术语对齐**
<p align="center">
  <img src=class/术语对齐.png width=800px/>
</p>


利用上述医学标准化术语库对结构化字典回复中的value与答案的相似度。

**生成式评测指标**
- Precision：结构化回复中字典的key与GT的key计算precision
- Recall：结构化回复中字典的key与GT的key计算Recall
- Bert score：结构化回复中字典的value与术语库对齐后与GT计算Bert score
