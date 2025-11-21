# 《普通高中英语课程标准》词汇表

## 简介

本项目旨在对中华人民共和国教育部制定的《普通高中英语课程标准（2017年版2020年修订）》附录词汇表进行拓展和量化分析。通过统计词汇表中所有单词的**首字母分布频率**，生成词频数据，从而深入了解高中英语教学大纲词汇的构成特点。

## 目标

1.  **数据清洗与分类：** 将课程标准词汇表中的所有单词，严格按照其首字母分类，并存入单独的文本文件。
2.  **词频统计：** 计算每个首字母下的词汇数量（即词频）。
3.  **构建基础数据集：** 为后续的词汇难度、重要性（`*` 或 `**` 标记）等更深入的教学研究提供结构化数据基础。

## 关键文件

本项目生成和使用的关键文件如下：

| 文件/目录名              | 类型     | 描述                                                         |
| ------------------------ | -------- | ------------------------------------------------------------ |
| `a.txt` - `z.txt`        | 文本文件 | **原始数据分类结果**。每个文件包含对应首字母开头的所有单词，每行一个单词，保留原始标记（`*`/`**`）。 |
| `word_count_summary.txt` | 文本文件 | **词频统计报告**。包含所有字母的词汇数量，并按数量降序排列，格式为 `字母 数量`。 |

## 统计结果摘要（按词汇数量降序）

根据当前的统计结果，词汇量最大的三个首字母分布如下：

1.  **S (348 词)**：占比最大。
2.  **C (307 词)**：位居第二。
3.  **P (229 词)**：位居第三。

## 统计与排序 (Shell 命令)

以下脚本的主要目的是统计当前目录下所有单字母 `.txt` 文件 (`a.txt` 到 `z.txt`) 的行数，并按行数从多到少排序，最后将结果保存到 `word_count_summary.txt` 文件中。

若数据源发生变动，可以使用以下命令重新生成 `word_count_summary.txt`：

```
# Ensure all text files from a.txt to z.txt exist
for file in {a..z}.txt; do
    # count the number of lines in the current file
    count=$(wc -l < "$file")
    # extract the single letter (a, b, c, ...) from the filename by removing the .txt suffix
    letter=$(basename "$file" .txt)
    # output the letter and the line count, separated by a space
    echo "$letter $count"
done | \
# sort the output: 
# -k2,2nr: sort by the second field (-k2,2), numerically (-n), in reverse order (-r), 
#   which means the highest count (most lines) comes first.
# > word_count_summary.txt: redirect the final sorted output to this file
sort -k2,2nr > word_count_summary.txt
```

| **命令/结构**                      | **用法说明**                                                 | **示例**                                         |
| ---------------------------------- | ------------------------------------------------------------ | ------------------------------------------------ |
| **`{a..z}.txt`**                   | **Bash 扩展/花括号扩展 (Brace Expansion)**：生成一个有序的字符串列表。在这里，它扩展为 `a.txt b.txt ... z.txt`。 | `{1..3}` $\rightarrow$ `1 2 3`                   |
| **`for file in ...; do ... done`** | **循环结构**：遍历 `in` 后面的列表中的每一个元素，并将其赋值给变量 `file`，然后执行 `do` 和 `done` 之间的命令。 |                                                  |
| **`wc -l`**                        | **`wc` (Word Count) 命令**：用于统计文件中的字节数、单词数或行数。`-l` 选项 specifically counts the **number of lines** (行数)。 | `wc -l my_file.txt`                              |
| **`< "$file"`**                    | **输入重定向**：将文件 `"$file"` 的内容作为 `wc -l` 命令的输入，而不是让 `wc` 直接读取文件并输出文件名。这使得输出只包含行数。 |                                                  |
| **`$(...)`**                       | **命令替换 (Command Substitution)**：执行圆括号中的命令，并将其标准输出作为字符串返回。在这里，它将行数赋值给变量 `count`。 | `today=$(date)`                                  |
| **`basename`**                     | **命令**：用于从文件路径中剥离目录信息或指定的后缀。在这里，`basename "$file" .txt` 移除文件名中的 `.txt` 后缀，只留下单个字母。 | `basename /path/to/a.txt .txt` $\rightarrow$ `a` |
| **`echo "$letter $count"`**        | **命令**：将变量的值和字符串输出到标准输出。这是管道 (`｜`) 的输入。 |                                                  |
| `&#124;`                           | **管道 (Pipe)**：将前一个命令（`echo`）的标准输出，连接并作为后一个命令（`sort`）的标准输入。 |                                                  |
| **`sort -k2,2nr`**                 | **`sort` 命令**：用于对输入内容进行排序。                    |                                                  |
|                                    | * **`-k2,2`**：指定排序键为第 2 字段（即行数），仅对第 2 字段进行排序。 |                                                  |
|                                    | * **`-n`**：以**数值 (numerical) 方式**进行排序，确保 10 在 2 后面，而不是按字符顺序（'10' 排在 '2' 前面）。 |                                                  |
|                                    | * **`-r`**：以**逆序 (reverse) 方式**进行排序，使得行数最多的排在最前面。 |                                                  |
| **`>`**                            | **输出重定向**：将左侧命令（`sort`）的标准输出写入到指定文件（`word_count_summary.txt`）中，**覆盖**原有内容。 |                                                  |

## 相关资料

-   [普通高中课程标准](https://www.ictr.edu.cn/download_center/put.html)
    -   [普通高中英语课程标准（2017年版2020年修订）](https://www.ictr.edu.cn/Uploads/File/2025/02/07/4.%E6%99%AE%E9%80%9A%E9%AB%98%E4%B8%AD%E8%8B%B1%E8%AF%AD%E8%AF%BE%E7%A8%8B%E6%A0%87%E5%87%86%EF%BC%882017%E5%B9%B4%E7%89%882020%E5%B9%B4%E4%BF%AE%E8%AE%A2%EF%BC%89.20250207211247.pdf)