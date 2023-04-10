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
