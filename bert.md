# 1. 개요

# 2. 명령어

## 2.1. 분류

### 2.1.1. 상세 분류(설명)

```python
BertForSequenceClassification.from_pretrained("bert-base-multilingual-cased", num_labels=num_labels)

b_input_ids, b_input_mask, b_labels = batch

outputs = model(b_input_ids, 
                        token_type_ids=None, 
                        attention_mask=b_input_mask, 
                        labels=b_labels)
```

- 라벨값은 반드시 0부터 1씩 증가하는 수열의 리스트가 되어야한다.
- ex
  `[0, 1, 2, 3, 4]` : 가능
  `[0, 1, 2, 5]`: 불가능
  `[1, 2, 3, 4, 5]`: 불가능
