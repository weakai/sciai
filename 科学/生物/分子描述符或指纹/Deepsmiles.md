---
title: DeepSMILES
---

An Adaptation of SMILES for Use in Machine-Learning of Chemical Structures

- 由于必须成对出现的SMILES语法元素，这些SMILES字符串在语法上通常是无效的

语法特点：

- 通过只使用闭合括号来避免不平衡括号的问题，其中括号的数量表示分支的长度。
- 通过在环闭合位置只使用一个符号来避免对环闭合符号的问题，符号表示环的大小。我们证明，通过字符串处理，可以将该语法转换为或转换为SMILES，而不会丢失任何信息，包括立体声构型。