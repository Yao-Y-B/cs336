# Stanford CS336 Language Modeling from Scratch | Spring 2026 | Lecture 1: Overview, Tokenization

## tokenization

what is text? unicode字符串

需要将 strings 编码成token
也需要将 token 解码成 string

### observation

- 许多单词 和单词前有一个空格 是不同的token
- 同一个单词 在开头和句子中被完全不同地表示
- 数字的token话是随机的

Compression ratio : bytes/token

string = “Hello, 🌍! 你好!”
10+4+6 = 20 bytes (emoji表情4bytes， 普通中文汉字一个3bytes)

indices = [13225,11,130321,235,0,220,117519,0]

8 tokens

so compression ratio = 2.5

可以通过增大词汇表的方式提高压缩率，但是会导致sparsity（稀疏性）

### character tokenizer 字符分词器

每个字符可以是一个 ord() 
一一对应也算一种

很多字符实际上很罕见， 所以利用链低， 压缩率也不是很好

### byte tokenizer
转化成byte序列，序列更长，但是都在0-255之间，vocab——size很小

### word tokenizer

先将字符串分块， 每一块叫做一个token 这样一个token有语义
缺点：
- 没有固定的词汇表
- 训练中没见过的新单词会有个UNKtoken


### bpe tokenizer

*byte pair encoding**: 最初的NLP用于神经翻译

basic idea：在原始文本上训练tokenizer去构建词汇表

1. 将字符串转为字节序列
2. 统计字节出现的次数
3. 找到出现次数最多的字节对
4. 合并最多的字节对， 一个新token代表这个对
5. 在原来字节对表中吧 新token代表的对 替换掉
6. 迭代 2-5 

得到的tokenizer可以用于其他字符串