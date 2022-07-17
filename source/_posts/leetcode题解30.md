---
title: leetcode题解30：串联所有单词的子串
date: 2022-01-25 18:30:03
tags:
    - leetcode
    - 滑动窗口
categories: 算法
mathjax: true
---

## 描述
该题来自于[力扣第30题](https://leetcode-cn.com/problems/substring-with-concatenation-of-all-words)

给定一个字符串`s`和一些长度相同的单词`words` 。找出`s`中恰好可以由`words`中所有单词串联形成的子串的起始位置。

注意子串要与`words`中的单词完全匹配，中间不能有其他字符 ，但不需要考虑`words`中单词串联的顺序。

<!--more-->

示例 1：

> 输入：s = "barfoothefoobarman", words = ["foo","bar"]
输出：[0,9]
解释：
从索引 0 和 9 开始的子串分别是 "barfoo" 和 "foobar" 。
输出的顺序不重要, [9,0] 也是有效答案。

示例 2：

> 输入：s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]
输出：[]

示例 3：

> 输入：s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]
输出：[6,9,12]


提示：
  * 1 <= s.length <= 104
  * s 由小写英文字母组成
  * 1 <= words.length <= 5000
  * 1 <= words[i].length <= 30
  * words[i]由小写英文字母组成


## 分析
需要知道words的一个组合是否是s的一个子串，由于words中的元素组合成的字符串是固定长度的，所以只需要考虑s中固定长度的子串，以实例2为例，words中每个词的长度为4，而words中元素个数也是4，所以组合成的字符串总长度为`4*4 = 16`，所以遍历s的长度为16的子串，看看该子串里的元素是否和words一样。具体过程为：
<font color='red'>wordgoodgoodgood</font>bestword -> w<font color='red'>ordgoodgoodgoodb</font>estword -> wo<font color='red'>rdgoodgoodgoodbe</font>stword -> ...

上述方法每次移动一个字符串，无法利用之前窗口的信息，显然不合理，由于每个元素长度为4，可以每次移动4个字符，即
* 以`w`开始，长度为16的窗口，以4为步长移动：<font color='red'>wordgoodgoodgood</font>bestword -> word<font color='red'>goodgoodgoodbest</font>word -> wordgood<font color='red'>goodgoodbestword</font> -> ...
* 以`o`开始，长度为16的窗口，以4为步长移动：w<font color='red'>ordgoodgoodgoodb</font>estword -> wordg<font color='red'>oodgoodgoodbestw</font>ord
* 以`r`开始以及以`d`开始相应的窗口移动

优化后的方法同样可以遍历完所有子串，而且由于每次移出一个单词，移入一个单词，从而可以很好的维护窗口中的元素。

在模拟窗口移动时发现可以继续优化，比如即将移入的元素不存在于words中，表明直到将该元素移出为止，不可能存在满足条件的子串。另外一种可以优化的情况，如果即将移入的元素在words中，但是元素个数超过了words中的该元素个数呢？显然这时候需要移出该元素前面的所有元素（包含自身），这样才能让该元素的个数少一个。

考虑了上面的两种情况就可以保证子串里的元素都在words中，且每个元素的个数不超过words中的元素，所以该窗口必为words中的某个组合，因为如果某个元素的个数小与words中的该元素个数，那么窗口的长度必然小于16。

## 代码

<details open>
<summary>python</summary>

```python
class Solution:
    def findSubstring(self, s: str, words: List[str]) -> List[int]:
        word_len = len(words[0])
        num_words = len(words)
        total_len = num_words * word_len
        s_len = len(s)
        all_indices = []

        if s_len < total_len:
            return all_indices

        word_map = {}
        for word in words:
            word_map[word] = word_map.get(word, 0) + 1

        for wi in range(min(word_len, s_len - total_len+1)):
            start = curr = 0
            window_map = {}
            while wi + curr * word_len <= (s_len - word_len):
                curr_word = s[wi+curr*word_len:wi+curr*word_len+word_len]
                if curr_word not in word_map:
                    window_map = {}
                    start = curr + 1
                else:
                    window_map[curr_word] = [
                        window_map.get(curr_word, [0, []])[0] + 1,
                        window_map.get(curr_word, [0, []])[1] + [curr]
                    ]

                    if window_map[curr_word][0] > word_map[curr_word]:
                        new_start = window_map[curr_word][1][0] + 1
                        for i in range(start, window_map[curr_word][1][0] + 1):
                            window_map[s[wi+i*word_len:wi+i*word_len+word_len]][0] -= 1
                            window_map[s[wi+i*word_len:wi+i*word_len+word_len]][1].pop(0)
                        start = new_start

                    if curr - start + 1 == num_words:
                        if window_map[curr_word][0] == word_map[curr_word]:
                            all_indices.append(wi+start*word_len)
                        window_map[s[wi+start*word_len:wi+start*word_len+word_len]][0] -= 1
                        start += 1

                curr += 1
        return all_indices
```
</details>
