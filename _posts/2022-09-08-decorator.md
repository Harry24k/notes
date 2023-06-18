---
layout: post
title: 파이썬에서의 골뱅이(@)의 의미
tags: [Python]
image:
  path: /images/abstract-1.jpg
  feature: abstract-1.jpg
---

이번 글에서는 파이썬에서의 골뱅이(@)의 의미에 대해 알아보겠습니다.

## Decorator

@는 파이썬에서 decorator라고 불리는 것으로, 한 함수를 다른 함수(=decorator)의 인자로 사용하고 싶을 때 사용합니다.

우선 아래와 같은 a와 b라는 함수가 있을 때,

`In[1]:`
```python
{% raw %}def a():
    print("A")
    
def b():
    print("B"){% endraw %}
```

`In[2]:`
```python
{% raw %}a(){% endraw %}
```
`Out[2]:`

    A
    

`In[3]:`
```python
{% raw %}b(){% endraw %}
```
`Out[3]:`

    B
    

그런데 나중에 a와 b 함수가 실행될 때의 시간을 출력해주고 싶다고 합시다.
그럼, 우선 아래와 같이 a와 b를 바꾸어주어야 합니다.

`In[4]:`
```python
{% raw %}from datetime import datetime
def a():
    print(datetime.now())
    print("A")
    
def b():
    print(datetime.now())
    print("B"){% endraw %}
```

`In[5]:`
```python
{% raw %}a(){% endraw %}
```
`Out[5]:`

    2022-09-07 09:50:14.681970
    A
    

`In[6]:`
```python
{% raw %}b(){% endraw %}
```
`Out[6]:`

    2022-09-07 09:50:14.710891
    B
    

하지만, decorator를 활용하면 아래와 같이 현재 시간을 출력하는 함수를 구성하여 간단하게 바꿀 수 있습니다.

`In[7]:`
```python
{% raw %}def print_now(f):
    def g():
        print(datetime.now())
        f()
    return g{% endraw %}
```

`In[8]:`
```python
{% raw %}@print_now
def a():
    print("A")

@print_now
def b():
    print("B"){% endraw %}
```

`In[9]:`
```python
{% raw %}a(){% endraw %}
```
`Out[9]:`

    2022-09-07 09:50:15.029385
    A
    

즉, decorator는 선언과 동시에 아래를 수행해주는 코드입니다.

`In[10]:`
```python
{% raw %}a = print_now(a){% endraw %}
```

## Decorator with arguments

그렇다면, decorator는 함수 인자말고도 다른 인자를 받을 수 있을까요?

이번에는 실행 시간의 형식을 인자에 따라 변환하고 싶다고 해보겠습니다.

`In[11]:`
```python
{% raw %}from datetime import datetime
def a():
    print(datetime.now())
    print("A"){% endraw %}
```

`In[12]:`
```python
{% raw %}a(){% endraw %}
```
`Out[12]:`

    2022-09-07 09:50:16.237676
    A
    

인자에 따라 위에처럼 시간까지 표기하거나,

`In[13]:`
```python
{% raw %}from datetime import datetime
def a():
    print(datetime.now().strftime("%Y-%m-%d"))
    print("A"){% endraw %}
```

`In[14]:`
```python
{% raw %}a(){% endraw %}
```
`Out[14]:`

    2022-09-07
    A
    

위에처럼 날짜만 표기하고 싶을 때는 decorator를 어떻게 구성해야할까요?

많은 방법이 있을 수 있지만, 가장 간단하게는 아래와 같이 class를 활용해서도 가능합니다.

`In[15]:`
```python
{% raw %}class PrintNow():
    def __init__(self, flag):
        self.flag = flag
    def __call__(self, f):
        def g():
            if self.flag==0:
                print(datetime.now())
            else: 
                print(datetime.now().strftime("%Y-%m-%d"))
            f()
        return g{% endraw %}
```

`In[16]:`
```python
{% raw %}@PrintNow(0)
def a():
    print("A")
a(){% endraw %}
```
`Out[16]:`

    2022-09-07 09:50:17.693655
    A
    

`In[17]:`
```python
{% raw %}@PrintNow(1)
def a():
    print("A")
a(){% endraw %}
```
`Out[17]:`

    2022-09-07
    A
    

이러한 decorator의 성질을 활용하면, 파이토치에서는 아래와 같이 gradient를 계산하지 않도록 decorator를 추가해줄 수 있습니다.

`In[18]:`
```python
{% raw %}import torch
@torch.no_grad()
def eval_accuracy(model, loader):
    for data in loader:
        model(...)
    pass{% endraw %}
```
