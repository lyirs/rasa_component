# rasa_component

rasa自定义组件

使用rasa版本3.3


### 稍做修改的jieba分词组件

在rasa shell nlu命令的输出中，可以显示词性标注的结果

为了实现这一功能，改用了jieba.posseg

扩展 rasa.nlu.tokenizers.tokenizer.Token 类以添加一个新属性来存储词性信息。用于在输出中观察到词性标注信息

（本来想使用自定义消息类覆盖掉原有的text_tokens字段，但失败了_(:з」∠)_）

分词组件是比较简单的自定义组件，可以照着官网的例子试着写写

使用：
```
  - name: components.jieba_tokenizer.JiebaTokenizer   #对应的文件夹.文件名.类名
    dictionary_path: "pipline/jieba_userdict"
```
23/5/17更新：
在训练数据中如果有中英文混杂的情况，经常会报这个错误：
```
Node: 'gradient_tape/rasa_sequence_layer_text/rasa_feature_combining_layer_text/concatenate_sparse_dense_features_text_sequence/ConcatOffset'
All dimensions except 2 must match. Input 1 has shape [63 8 768] and doesn't match input 0 with shape [63 10 128].
         [[{{node gradient_tape/rasa_sequence_layer_text/rasa_feature_combining_layer_text/concatenate_sparse_dense_features_text_sequence/ConcatOffset}}]] [Op:__inference_train_function_56448]  
```
来自Rasa的Issue： https://github.com/RasaHQ/rasa/issues/7910
```
JiebaTokenizer is meant for Chinese only text. When multiple languages are used in the same sentence, the tokenizer adds an extra whitespace token in between two chinese and english tokens. For example,
text: 如何才能在下载和安装google app
Tokens output by the tokenizer will be: ['如何', '才能', '在', '下载', '和', '安装', ' ', 'google', 'app']
```
目前采用在tokenize中：在生成 Token 对象之前，先检查分词的结果是否只包含空格。如果是，那么就跳过这个分词，不生成对应的 Token 对象。然后，使用 find 方法来查找每个分词在原始文本中的实际位置，而不是简单地使用 jieba 分词器生成的位置。
这个解决方案仍然是一个折衷的方案，它可能无法处理所有的边缘情况。如果你发现这个解决方案仍然无法满足你的需求，那么你可能需要寻找一个更适合处理中英文混合文本的分词器，或者直接修改 jieba 分词器的源代码。

### 数字提取组件

不想使用DucklingEntityExtractor就用这个吧_(:з」∠)_

```
  - name: components.custom_number_extractor.CustomNumberExtractor
```

### 时间提取组件
```
  - name: components.custom_time_extractor.CustomTimeExtractor
```
支持格式：

HH:mm

hh:mm A/AM/PM

hh:mm A/AM/PM (with optional seconds)

ISO 8601 time format with an optional timezone (e.g., "12:34:56+08:00")
