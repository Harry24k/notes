---
layout: post
title: Jupyter notebook 이름 가져오기
tags: [Python, Jupyter]
image:
  path: /images/abstract-1.jpg
  feature: abstract-1.jpg
---

이번 글에서는 현재 Jupyter notebook의 이름을 가져오는 방법에 대해 다루어보겠습니다.

## Magic Function

우선 먼저 Jupyter notebook에서 지원하는 *Magic function* 이라는 것에 대해 알아볼 필요가 있습니다.

Magic function이란, 흔히 Jupyter notebook에서 "%"로 시작되는 명령으로, Python 언어가 아니라 Jupyter notebook 에서 지원하는 함수들로 "%" 이하의 코드들을 실행하는 것을 말합니다.

간단히 예를들면, 흔히 우리가 사용했던 아래와 같은 유명한 Magic function이 있겠습니다.

`In[1]:`
```python
{% raw %}%matplotlib inline{% endraw %}
```

위 역시 "%"로 시작되는 명령이기 때문에 Magic function이며, 그 의미는 Jupyter notebook에서 지원하는 matplotlib의 함수를 사용하는 데, 그 중에서도 inline이라는 옵션을 주어 matplotlib과 jupyter notebook을 interactive하게 연결시키겠다는 것 입니다.

[지난 글](https://harry24k.github.io/share/)에서 다루었던 store 역시 magic function에 속합니다.

`In[2]:`
```python
{% raw %}a = 5
%store a{% endraw %}
```
`Out[2]:`

    Stored 'a' (int)
    

더 많은 Magic function은 아래 주소에서 찾아볼 수 있습니다.

[https://ipython.readthedocs.io/en/stable/interactive/magics.html](https://ipython.readthedocs.io/en/stable/interactive/magics.html)

## Single Percent v.s Double Percent

하지만, "%"에는 제한점이 하나 있는데 바로 "%"로 시작하는 그 줄에만 효과가 있다는 것입니다. 즉, 다시 말하면 "%"는 해당 코드 줄에만 영향을 미칠 뿐, 셀 전체에 영향을 미치지 않습니다.
그렇기에 우리는 아래와 같은 코드를 실행시킬 수 있습니다.

`In[3]:`
```python
{% raw %}%matplotlib inline
import matplotlib{% endraw %}
```

그렇다면 여러 줄의 Magic function을 사용하려면 어떻게 해야할까요?

정답은 "%%"를 사용하는 것입니다. "%%"는 해당 셀이 Magic function이라는 것을 의미합니다.

`In[4]:`
```javascript
{% raw %}%%javascript
element.append("Hi")
element.append("&")
element.append("Hello"){% endraw %}
```
`Out[4]:`


    <IPython.core.display.Javascript object>


위와 같은 경우 Jupyter notebook에서 제공하는 javascript로 해당 셀을 실행시키겠다는 의미이며, 밑의 세 줄이 모두 javascript로 실행이 됩니다. 따라서, 이 경우 같은 셀에 Python을 실행할 수 없습니다.

`In[5]:`
```javascript
{% raw %}%%javascript
element.append("Hi")
element.append("&")
element.append("Hello")
import matplotlib{% endraw %}
```
`Out[5]:`


    <IPython.core.display.Javascript object>


[https://ipython.readthedocs.io/en/stable/interactive/tutorial.html#magics-explained](https://ipython.readthedocs.io/en/stable/interactive/tutorial.html#magics-explained)

## 주피터 노트북 이름 얻기

위와 같은 javascript magic function을 활용하면, 손쉽게 현재 jupyter notebook의 이름을 가져올 수 있습니다.

`In[6]:`
```javascript
{% raw %}%%javascript 
IPython.notebook.kernel.execute('nb_name = "' + IPython.notebook.notebook_name + '"'){% endraw %}
```
`Out[6]:`


    <IPython.core.display.Javascript object>


`In[7]:`
```python
{% raw %}nb_name{% endraw %}
```
`Out[7]:`




    'jupytername.ipynb'



다만 위와 같은 Magic function의 경우 Juypter notebook의 Kernel 명령 중 "Restart & Run All"과는 호환이 안되는 문제가 있습니다. 이에 관해서는 해결이 불가능하며, 자세한 내용은 아래 주소를 참고 바랍니다.

[https://stackoverflow.com/questions/30745228/executing-javascript-cells-in-jupyter-notebooks](https://stackoverflow.com/questions/30745228/executing-javascript-cells-in-jupyter-notebooks)
