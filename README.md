# 藏文缩写词公开数据集

**Tibetan Abbreviations Public Dataset**

本数据集用于藏文缩写词还原与识别研究，包含藏文缩写对照语料和藏文字符集，可用于相关方法的训练与评测。对照语料由缩写原文与对应的还原文本组成，两个文件按相同行号一一对应。

This dataset supports research on Tibetan abbreviation restoration and recognition. It contains 10,820 line-aligned abbreviation–restoration pairs and a 7,387-entry Tibetan character inventory, including composite forms.

## 数据来源

数据整理自谢拉编著的《藏文缩略语词典》（民族出版社），并参考项布旺杰的《缩略语词典》（四川民族出版社）。

## 文件说明

| 文件 | 内容 | 记录数 | 编码 |
| --- | --- | ---: | --- |
| [data/Source.txt](data/Source.txt) | 藏文缩写原文，作为输入文本 | 10,820 行 | UTF-8，带 BOM |
| [data/Restore.txt](data/Restore.txt) | 与缩写原文对应的还原文本，作为参考文本 | 10,820 行 | UTF-8，带 BOM |
| [data/tibetan_VCC_7387.txt](data/tibetan_VCC_7387.txt) | 藏文字符集，包含字符及组合形式 | 7,387 条 | UTF-8，无 BOM |
| [dataset_manifest.json](dataset_manifest.json) | 数据版本、来源、文件大小、记录数及 SHA-256 校验值 | — | UTF-8 |

`Source.txt` 第 n 行与 `Restore.txt` 第 n 行构成一条“藏文缩写 → 还原文本”记录。两个文件均无空行，使用时应保持原始顺序，避免分别排序或删除其中一侧的记录。这里的 10,820 对为原始行级记录数；一条原始记录中可以含有多个句段。

字符集文件的 7,387 条为文件记录数，其中有 7,199 个不同条目，包含重复项以及由多个 Unicode 码点组成的组合形式。该记录数不直接等于模型词表大小。

## 读取示例

三份数据文件均使用 CRLF 换行，末行没有额外换行符。建议使用 `splitlines()` 读取与统计记录；对照语料使用 `utf-8-sig` 编码读取，以处理文件开头的 BOM。

```python
from pathlib import Path

data_dir = Path("data")
sources = (data_dir / "Source.txt").read_text(encoding="utf-8-sig").splitlines()
restores = (data_dir / "Restore.txt").read_text(encoding="utf-8-sig").splitlines()
assert len(sources) == len(restores)
pairs = list(zip(sources, restores))

characters = (
    (data_dir / "tibetan_VCC_7387.txt").read_text(encoding="utf-8").splitlines()
)
```

本次发布保留作者提供文件的全部原始字节，未做去重、重排、分句或 Unicode 规范化。进行训练与评测时，建议先确定固定划分，并让同一原始记录及重复文本关联样本保留在同一划分中。

## 下载与版本

初始公开版本：**v1.0.0**。三份数据文件可直接从 `data/` 下载，也可从 [Releases](https://github.com/rindon520/tibetan-abbreviations-public-dataset/releases) 获取作者提供的原始压缩包。

```bash
git clone https://github.com/rindon520/tibetan-abbreviations-public-dataset.git
```

原始压缩包 `Tibetan Abbreviations Public Dataset.zip` 的 SHA-256：

```text
18e77950851745f52e9d579f15402a51392a4609a2751a16afe49c27ab8806a2
```

逐文件校验值见 [dataset_manifest.json](dataset_manifest.json)。引用时可记录数据集名称、版本和仓库地址，以明确所使用的数据版本。当前仓库尚未指定独立的数据使用许可证。
