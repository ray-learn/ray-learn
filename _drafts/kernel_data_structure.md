---
title: "知识点-内核数据结构"
---

## 内核数据结构
大多数内核数据结构都占据固定长度的表而不是动态地分配空间，这一方法的优点是内核代码简单。但是它限制了一种数据结构的表项的数据，为系统生成时原始配置的数目。如果在系统操作期间，内核用完了一种数据结构的表项，则它不能动态地为新的表项分配空间，而是必须向发出请求的用户报告一个错误。另一方面，如果内核被配置得具有不可能用完的表空间，则由于不能用于其他目的而使多余的表空间浪费了。然而，一般都认为内核算法的简单性比挤出主存中每一个仅有的字节的必要性更重要一些。算法通常使用简单的循环，来寻找表中的空闲表项，这是一个较易于理解的方法，而且有时比复杂的分配方案更为有效。