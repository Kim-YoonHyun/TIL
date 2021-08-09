#  파이썬 주요 내용 정리

## class

- 객체지향 - 인스턴스, 정적메소드 등

```python
class TestClass:
    def __init__(self, name, age, money):
        self.name = name
        self.age = age
        self.__money = money
    
    @staticmethod
    def calculate(amount):
        return amount*100000
    
    def make_money(self, amount):
        return self.__money += amount
    
    def __str__(self):
        return f'이름: {self.name}, 나이: {self.age}, 돈: {self.__money:,d}'
```

```python
alice = TestClass('alice', 27, 10000000)
```

```python
extra_money = TestClass.calculate(100)
```

```python
alice.make_money(extra_money)
print(alice)
```

- *@staticmethod* 를 통해 **정적 메소드** 입력 가능
- 클래스 상속시 *@subtractmethod*
- 아직까지 class 의 적절한 사용 방법, 사용 시기 등 연습이 필요할 것으로 생각됨.

## numpy

- numpy는 vector, matrix, tensor 등의 수치적 표현이 가능하며 연산에 있어 매우 유용한 명령어임
- 전부 외우기 힘들 정도로 다양한 명령어가 존재하며 이는 계속해서 사용해보며 익혀나가야함

| numpy 명령어 | 기능      |
| ------------ | --------- |
| add          | 더하기    |
| subtract     | 빼기      |
| dot          | 행렬 연산 |
| ....         |           |

[numpy 명령어 정리 사이트](https://www.datacamp.com/community/blog/python-numpy-cheat-sheet)





