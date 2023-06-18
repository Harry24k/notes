---
layout: post
title: 파이썬에서의 밑줄(_)의 의미
tags: [Python]
image:
  path: /images/abstract-1.jpg
  feature: abstract-1.jpg
---

이번 글에서는 파이썬에서의 밑줄(_)의 의미에 대해 알아보겠습니다.

## Underscore

_는 파이썬에서 underscore라고 불리는 것으로, 다양한 쓰임새가 존재합니다.

이번 글에서는 그 중 아래와 같은 세 가지의 주된 사용 방법에 대해 다루어 보려합니다.

1. 단독 선언 

2. Private 변수, 함수 및 클래스의 선언

3. 매직 메소드의 선언

### 1. 단독 선언

가장 단순한 방법은 underscore 그 자체를 실행시켜보는 것일 겁니다.

`In[1]:`
```python
{% raw %}_{% endraw %}
```
`Out[1]:`




    ''



신기하게도 에러가 나지 않고 정상적으로 ''를 반환합니다.

이는 underscore가 기본적으로 파이썬 인터프리터에서 가장 최근의 출력 값을 나타내기 때문입니다. 예를 들면 아래와 같습니다.

`In[2]:`
```python
{% raw %}a = 10
b = 20
a + b{% endraw %}
```
`Out[2]:`




    30



`In[3]:`
```python
{% raw %}_{% endraw %}
```
`Out[3]:`




    30



즉, underscore를 단독으로 사용할 경우 가장 최근의 출력값을 가져올 수 있습니다. 다만 print 등의 화면 출력 함수로는 underscore 값이 바뀌지 않습니다.

`In[4]:`
```python
{% raw %}print(b+30)  # print 함수 호출{% endraw %}
```
`Out[4]:`

    50
    

`In[5]:`
```python
{% raw %}_  # 이전 출력 값인 30을 유지{% endraw %}
```
`Out[5]:`




    30



`In[6]:`
```python
{% raw %}a  # a 호출{% endraw %}
```
`Out[6]:`




    10



`In[7]:`
```python
{% raw %}_  # a 호출로 인해 최근 출력값 바뀜{% endraw %}
```
`Out[7]:`




    10



물론, underscore 자체를 변수로써 활용할 수 있습니다. 이 방법 중 가장 널리 쓰이는 방법이 바로 **무의미한 인자를 전달**받을 때 underscore로 받는 것입니다.

`In[8]:`
```python
{% raw %}output, _ = ["a", "b"]{% endraw %}
```

위의 결과 값은 아래와 같습니다.

`In[9]:`
```python
{% raw %}output, _{% endraw %}
```
`Out[9]:`




    ('a', 'b')



이를 활용하면 반복문에서 필요한 것만 따와서 사용할 수 있습니다.

`In[10]:`
```python
{% raw %}for n, _ in enumerate(["a", "b", "c"]):
    print(n){% endraw %}
```
`Out[10]:`

    0
    1
    2
    

물론, 이와 같은 활용법의 경우 단순히 _라는 이름의 객체를 받는 인자로 사용한 것으로 underscore 대신 temp 등을 사용하는 것과 동일합니다. 다만, underscore 자체는 단독으로 잘 사용하지도 않고, 쓰기 간편한 이름이기 때문에 이처럼 사용되는 것입니다.

실제로 _는 일반 객체와 같이 사용 가능하며, 위의 반복문의 경우에도 계속 값이 할당되고 있습니다.

`In[11]:`
```python
{% raw %}for n, _ in enumerate(["a", "b", "c"]):
    print(n, _){% endraw %}
```
`Out[11]:`

    0 a
    1 b
    2 c
    

`In[12]:`
```python
{% raw %}_ + " is not _"{% endraw %}
```
`Out[12]:`




    'c is not _'



조금 더 활용법을 높이면 한 가지의 인자 뿐만이 아니라 여러 인자를 대체할 수도 있습니다.

`In[13]:`
```python
{% raw %}first, *_, last = ["a", "b", "c", "d"]{% endraw %}
```

`In[14]:`
```python
{% raw %}first, last{% endraw %}
```
`Out[14]:`




    ('a', 'd')



마찬가지로 이는 underscore가 아니더라도 가능합니다.

`In[15]:`
```python
{% raw %}first, *middle, last = ["a", "b", "c", "d"]
middle{% endraw %}
```
`Out[15]:`




    ['b', 'c']



### 2. Private 변수, 함수 및 클래스 선언

사실상 underscore의 가장 큰 활용법을 꼽자면 private한 변수, 함수 및 클래스를 만드는 것이라고 하고 싶습니다.

우선 하나의 underscore를 쓰는 **single underscore**에 대해 알아보겠습니다.

PEP 8 Style Guide에서는 python에서의 single underscore에 대해 아래와 같이 설명합니다.

`_single_leading_underscore`: weak "internal use" indicator. E.g. `from M import *` does not import objects whose name starts with an underscore.

실제로 underscore가 붙은 아래와 같이 utils.py가 있고, 이를 호출한다하면,

`In[16]:`
```python
{% raw %}# utils.py
def hi():
    print("hi")
    
def _hello():
    print("hello"){% endraw %}
```

`In[17]:`
```python
{% raw %}# main.py
from utils import *
hi()
_hello(){% endraw %}
```
`Out[17]:`

    hi
    


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-16-dfe85723a031> in <module>
          2 from utils import *
          3 hi()
    ----> 4 _hello()
    

    NameError: name '_hello' is not defined


import *로는 underscore로 시작하는 함수는 불러오지 않게됩니다. 그러나, 아래와 같이 직접 호출하는 것은 가능합니다.

`In[18]:`
```python
{% raw %}import utils
utils._hello(){% endraw %}
```
`Out[18]:`

    hello
    

이러한 이유로 사용자가 알고 싶게 하지 않거나, 변경될 우려가 있는 변수나 함수들의 경우 underscore로 시작하는 이름을 부여합니다.

`In[19]:`
```python
{% raw %}class A:
    def hi(self):
        print("hi")
    def _hello(self):
        print("hello"){% endraw %}
```

`In[20]:`
```python
{% raw %}a = A()
a.hi()  # hi는 사용자들이 접근 가능하며 자주 쓸 수 있는 함수.
a._hello()  # hello는 사용자들이 접근은 가능하지만 변경될 우려가 있어 쓰지는 않았으면 하는 함수.{% endraw %}
```
`Out[20]:`

    hi
    hello
    

그렇다면 두 개의 underscore를 쓰면 어떻게 될까요?

이는 **double underscore (Name Mangling)**라고 불리며, 파이썬 문서에 의하면 아래와 같습니다.

Any identifier of the form `__spam` (at least two leading underscores, at most one trailing underscore) is textually replaced with `_classname__spam`, where `classname` is the current class name with leading underscore(s) stripped. This mangling is done without regard to the syntactic position of the identifier, so it can be used to define class-private instance and class variables, methods, variables stored in globals, and even variables stored in instances. private to this class on instances of other classes.

즉, double underscore는 실제로 사용자가 접근을 못하게 하는 strong한 private 변수 및 함수를 만드는 데에 있으며, 실제로는 `_classname__name`으로 접근을 해야합니다.

`In[21]:`
```python
{% raw %}class A:
    def hi(self):
        print("hi")
    def _hello(self):
        print("hello")
    def __bye(self):
        print("bye"){% endraw %}
```

`In[22]:`
```python
{% raw %}a = A()
a.__bye(){% endraw %}
```
`Out[22]:`


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-21-1e723cf722d0> in <module>
          1 a = A()
    ----> 2 a.__bye()
    

    AttributeError: 'A' object has no attribute '__bye'


실제로 double underscore는 위와 같이 함수 이름으로는 접근 불가능하며, 아래와 같이 접근 가능합니다.

`In[23]:`
```python
{% raw %}a._A__bye() {% endraw %}
```
`Out[23]:`

    bye
    

이 경우 상속 역시 double underscore는 상속 받지 못하며, 위와 동일한 방법으로 접근해야합니다.

`In[24]:`
```python
{% raw %}class B(A):
    ...{% endraw %}
```

`In[25]:`
```python
{% raw %}b = B()
b.__bye(){% endraw %}
```
`Out[25]:`


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-24-bfeaa232428e> in <module>
          1 b = B()
    ----> 2 b.__bye()
    

    AttributeError: 'B' object has no attribute '__bye'


`In[26]:`
```python
{% raw %}b._A__bye(){% endraw %}
```
`Out[26]:`

    bye
    

`In[27]:`
```python
{% raw %}b.__B__bye(){% endraw %}
```
`Out[27]:`


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-26-1d96c611a92d> in <module>
    ----> 1 b.__B__bye()
    

    AttributeError: 'B' object has no attribute '__B__bye'


### 3. 매직 메소드의 선언

마지막으로는 클래스와 관련된 사용법으로, 매직 메소드(magic method) 혹은 던더 메소드(dunder method)라고 불리는 것입니다.

`In[28]:`
```python
{% raw %}class A():
    def __init__(self):
        a = "1"{% endraw %}
```

매직 메소드는 양쪽으로 double underscore가 존재하며, 흔히 사용되는 위의 `__init__`가 대표적인 매직 메소드입니다.

매직 메소드는 파이썬에서 기본적으로 제공하는 object에 내포되어있는 함수들입니다. 쉽게 설명하면, 파이썬 제공 함수 및 연산자가 접근하는 내부에서 호출되는 함수입니다.

`In[29]:`
```python
{% raw %}object.__init__{% endraw %}
```
`Out[29]:`




    <slot wrapper '__init__' of 'object' objects>



위에서 보듯 object는 기본적으로 `__init__`이라는 매직 메소드를 가지고 있음을 확인할 수 있으며,

`In[30]:`
```python
{% raw %}float.__add__{% endraw %}
```
`Out[30]:`




    <slot wrapper '__add__' of 'float' objects>



float 역시 기본적으로 `__add__`라는 매직 메소드를 가지고 있습니다.

즉, 파이썬에서 실질적으로 가장 built-in 함수라고 할 수 있으며, 이 함수들을 기반으로 다양한 함수가 파생될 수 있습니다.

#### `__init__` v.s. `__new__`

우선 클래스 선언에서 가장 많이 쓰이는 `__init__`에 대해 알아보도록 하겠습니다.

`__init__`은 흔히 알려진 바와 같이 파이썬에서 객체가 생성되었을 때 내부 속성을 초기화해주는 역할을 합니다.

`In[31]:`
```python
{% raw %}class Cat():
    def __init__(self):
        self.sound = "meow"
        
cat = Cat()
cat.sound{% endraw %}
```
`Out[31]:`




    'meow'



이 때문에 종종 생성자(constructor)로 오인 받으나, 실제로는 그렇지 않습니다.

실제로 객체를 생성하는 역할은 `__new__`라는 함수이며, 보이지 않게끔 아래와 같이 실행이 됩니다.

`In[32]:`
```python
{% raw %}class Cat():
    def __new__(cls):
        print("__new__ called")
        return super().__new__(cls)
        
    def __init__(self):
        print("__init__ called")
        self.sound = "meow"
        
cat = Cat()
cat.sound{% endraw %}
```
`Out[32]:`

    __new__ called
    __init__ called
    




    'meow'



즉, `__new__` 함수를 통해 새로운 객체가 생성되며, `__init__`을 통해 생성된 객체의 속성을 초기화하게 됩니다.

`__init__`에 인자가 전달되는 경우에도 마찬가지로 작동하며, 자세한 건 아래와 같습니다.

`In[33]:`
```python
{% raw %}class Animal():
    def __new__(cls, is_cat):
        print("__new__ called")
        print("is cat?:", is_cat)
        return super().__new__(cls)
        
    def __init__(self, is_cat):
        print("__init__ called")
        if is_cat:
            self.sound = "meow"
        else:
            self.sound = "woof"
        
cat = Animal(is_cat=True)
cat.sound{% endraw %}
```
`Out[33]:`

    __new__ called
    is cat?: True
    __init__ called
    




    'meow'



이를 활용하면, 클래스가 선언될 당시 어떤 인자를 전달 받았느냐에 따라서 다른 클래스를 반환할 수도 있습니다.

`In[34]:`
```python
{% raw %}class Animal():
    def __new__(cls, is_cat):
        print("__new__ called")
        if is_cat:
            return Cat()
        else:
            return Dog()

class Cat():
    def __init__(self):
        self.sound = "meow"
        
class Dog():
    def __init__(self):
        self.sound = "woof"
        
cat = Animal(is_cat=True)
cat.sound{% endraw %}
```
`Out[34]:`

    __new__ called
    




    'meow'



`In[35]:`
```python
{% raw %}cat.__class__{% endraw %}
```
`Out[35]:`




    __main__.Cat



이를 응용하면 다음과 같은 코딩도 가능합니다

`In[36]:`
```python
{% raw %}class Grow():
    def __new__(cls, cat):
        cat.age = 20
        return cat
        
class Cat():
    def __init__(self):
        self.sound = "meow"
        self.age = 1{% endraw %}
```

`In[37]:`
```python
{% raw %}cat = Cat()
cat = Grow(cat)
cat.sound, cat.age{% endraw %}
```
`Out[37]:`




    ('meow', 20)



#### `__str__` v.s. `__repr__`

`__str__`은 말 그대로 str 함수로 해당 객체가 전달되었을 때 어떤 것을 출력할 것인지를 설정하는 함수입니다. 

`In[38]:`
```python
{% raw %}class Cat():
    def __init__(self):
        self.sound = "meow"
        self.age = 1
    
    def __str__(self):
        return "I'm a cat."{% endraw %}
```

`In[39]:`
```python
{% raw %}cat = Cat()
str(cat){% endraw %}
```
`Out[39]:`




    "I'm a cat."



하지만 아래와 같은 코드도 동일하게 동작합니다.

`In[40]:`
```python
{% raw %}class Cat():
    def __init__(self):
        self.sound = "meow"
        self.age = 1
    
    def __repr__(self):
        return "I'm a cat."{% endraw %}
```

`In[41]:`
```python
{% raw %}cat = Cat()
str(cat){% endraw %}
```
`Out[41]:`




    "I'm a cat."



정확히 말하자면 `__repr__`는 객체를 표현할 때 호출되는 함수이며, `__str__`은 객체를 문자화할 때 호출되는 함수입니다.

아래 예시에서 단번에 이해할 수 있습니다.

`In[42]:`
```python
{% raw %}class Cat():
    def __init__(self, age):
        self.age = age
    
    def __str__(self):
        return "I'm a cat."
    
    def __repr__(self):
        args = ", ".join([k+"="+str(v) for k, v in cat.__dict__.items()])  # __dict__: 해당 객체의 속성 정보
        return f"{self.__class__.__name__}({args})"  # __class_.__name__: 해당 객체의 class 이름{% endraw %}
```

`In[43]:`
```python
{% raw %}cat = Cat(age="1"){% endraw %}
```

`In[44]:`
```python
{% raw %}print(cat)  # __str__ 호출{% endraw %}
```
`Out[44]:`

    I'm a cat.
    

`In[45]:`
```python
{% raw %}cat  # __repr__ 호출{% endraw %}
```
`Out[45]:`




    Cat(age=1)



`__str__`이 없는 경우에는 `__repr__`를 참조하지만, 반대의 경우인 `__repr__`이 없는 경우에는 `__str__`을 `__repr__`로 사용하지는 않습니다.

`In[46]:`
```python
{% raw %}class Cat():
    def __str__(self):
        return "Cat()"
    
class Dog():
    def __repr__(self):
        return "Dog()"{% endraw %}
```

`In[47]:`
```python
{% raw %}cat = Cat(){% endraw %}
```

`In[48]:`
```python
{% raw %}print(cat){% endraw %}
```
`Out[48]:`

    Cat()
    

`In[49]:`
```python
{% raw %}cat{% endraw %}
```
`Out[49]:`




    <__main__.Cat at 0x1ce7b774f10>



`In[50]:`
```python
{% raw %}dog = Dog(){% endraw %}
```

`In[51]:`
```python
{% raw %}print(dog){% endraw %}
```
`Out[51]:`

    Dog()
    

`In[52]:`
```python
{% raw %}dog{% endraw %}
```
`Out[52]:`




    Dog()



강제되는 것은 아니나, `__repr__` 함수의 경우, `eval` 함수에 입력되었을 때 실행되도록 하게 구현하는 것이 추천됩니다.

`In[53]:`
```python
{% raw %}new_dog = eval(dog.__repr__())
id(dog), id(new_dog){% endraw %}
```
`Out[53]:`




    (1986346306720, 1986345599328)



#### `__slots__`

`__slots__`는 객체의 새로운 속성 추가를 제한하여 메모리 사용량을 개선하기 위한 함수입니다.

일반적인 클래스라면, 아래와 같이 새로운 속성을 추가할 때는 `class.attribute`의 형태로 추가할 수 있습니다.

`In[54]:`
```python
{% raw %}class Cat():
    def __init__(self):
        self.age = 1{% endraw %}
```

`In[55]:`
```python
{% raw %}cat = Cat()
cat.age{% endraw %}
```
`Out[55]:`




    1



`In[56]:`
```python
{% raw %}cat.sound = "meow"{% endraw %}
```

`In[57]:`
```python
{% raw %}cat.sound{% endraw %}
```
`Out[57]:`




    'meow'



실제로 객체의 딕셔너리에서 추가된 정보를 발견할 수 있습니다.

`In[58]:`
```python
{% raw %}cat.__dict__{% endraw %}
```
`Out[58]:`




    {'age': 1, 'sound': 'meow'}



`__slots__`를 활용하게 되면, 이러한 추가가 불가능해집니다.

`In[59]:`
```python
{% raw %}class Cat():
    __slots__ = ['age']
    def __init__(self):
        self.age = 1{% endraw %}
```

`In[60]:`
```python
{% raw %}cat = Cat()
cat.age{% endraw %}
```
`Out[60]:`




    1



`In[61]:`
```python
{% raw %}cat.sound = "meow"{% endraw %}
```
`Out[61]:`


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-60-3c974ec3f786> in <module>
    ----> 1 cat.sound = "meow"
    

    AttributeError: 'Cat' object has no attribute 'sound'


추가적으로 `__dict__`로도 접근 불가능하게 됩니다.

`In[62]:`
```python
{% raw %}cat.__dict__{% endraw %}
```
`Out[62]:`


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-61-d71cdcc6af69> in <module>
    ----> 1 cat.__dict__
    

    AttributeError: 'Cat' object has no attribute '__dict__'


#### 번외. `__all__`

`__all__`은 파이썬 클래스 내에서 선언되지는 않지만, 간혹 패키지들을 살펴보면 볼 수 있는 던더메소드입니다.

보통 `__init__.py` 파일에서 `__all__`을 정의하는 데, 이는 곧 `import *`가 호출되었을 때 `__all__`에 있는 모든 함수를 호출하겠다는 의미입니다. 

단, 이 때 아래와 같이 `__all__`에 함수 및 클래스를 String 형식으로 전달해줍니다.

`In[63]:`
```python
{% raw %}# __init__.py
__all__ = ['Cat']{% endraw %}
```
