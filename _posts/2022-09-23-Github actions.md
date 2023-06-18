---
layout: post
title: Github actions 사용하기
tags: [Python]
image:
  path: /images/abstract-1.jpg
  feature: abstract-1.jpg
---

이번 글에서는 Github에서 제공하는 Actions를 사용해보도록 하겠습니다.

## Continous Integration (CI)

Actions를 사용하기 이전에, 우선 지속적 통합이라고도 불리는 CI에 대해서 알아보겠습니다.

CI는 말그대로, 지속적으로 나열된 과정들을 자동화하는 것을 의미합니다.

보통 Github을 사용하게 된다면 아래와 같은 상황을 맞이하게 됩니다.

1. 기능 구상 및 버그 발견

2. 코드 수정

3. 코드 동작 확인

4. Github Push

5. Branch Merge

6. 코드 동작 확인

물론, 코드 수정을 할 일이 아예 없거나 (=1회성 배포) 혹은 짧은 기간 동안에만 활용될 코드라면 위 과정을 매번 해줘도 상관 없지만, 문제는 _지속적_으로 코드를 배포하려할 경우 1-2번에 해당하는 수정 작업보다 3-6번에 해당하는 점검부분이 더 오래 걸릴 수도 있습니다.

따라서, 이 3-6번을 좀 더 효율적으로 처리하기 위해 자동화하는 것을 CI라고 할 수 있습니다.

CI와 함께 사용되는 개념인 Continuous Deployment (CD)는 6번 이후에 진행되는 배포 과정을 자동화하는 것을 의미하며, CI와 CD는 함께 진행되는 경우도 많기 때문에 CI/CD로 통합하여 부르기도 합니다.

## Github Actions

Github Actions는 이러한 CI/CD를 자동화하는 것을 지원하는 것뿐만 아니라, Github의 Issue, Pull 등을 자동으로 처리해줄 수 있는 기능을 제공합니다.

우선, Actions에서는 특정 이벤트마다 실행될 수 있는 Workflow를 정의할 수 있습니다. 주로 작업 단위로 Workflow를 설정하며, 예를 들면 "Push를 할 때마다 점검하기", "Issue가 등록될 때마다 Tag해주기" 등이 될 수 있습니다.

Workflow는 "Repo 이름/.github/workflows/" 폴더에 .yml 파일을 생성하여 사용할 수 있으며, yml 파일 안에 있는 내용을 바탕으로 Workflow가 실행됩니다.

`In[1]:`
```ruby
{% raw %}# test.yml
name: test  # 이름 설정

on:  # Workflow가 실행될 조건 설정
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]
        
jobs:  # Workflow의 세부 작업 설정
  build-ubuntu:  # build-ubuntu라는 작업 설정
    runs-on: ubuntu-latest  # build-ubuntu가 구동될 환경 명명
    steps:  # ubuntu에서 실행될 순차적 단계 설정
      - name: Say hello  # Say hello라는 단계
        run: 
            echo "Hello!"  # Hello 출력
...{% endraw %}
```

yml은 여러 인자를 받을 수 있으나, 간단하게만 설명드리면 위와 같이 이름(name)이나 실행될 트리거(on)을 설정할 수 있습니다.

위의 경우에는 name이 test인 workflow이며, master brnche에 pus가 되거나 pull이 될 때 그 아래에 있는 작업들(jobs)을 실행한다는 것입니다.

jobs 안에는 여러 job이 생성될 수 있으며, 위 코드에서는 "build-ubuntu"라는 job이 하나 들어 있는 것입니다. 다만, 기본적으로 job들은 서로 dependency가 설정되지 않으면 parallel하게 진행됩니다. 

각 job은 순차적으로 실행되는 steps로 이루어져 있으며, 그 이전에 어느 환경에서 돌아갈건지 "runs-on"이라는 인자로 설정 가능합니다.

그 이후에는 자동적으로 steps를 차례로 진행하게 되고, 이를 통해 특정 조건마다 같은 작업을 Github에서 반복할 수 있습니다.

보다 자세한 문법은 아래에서도 자세히 설명하고 있으니 참고 부탁드립니다.

* https://docs.github.com/en/actions
* https://zzsza.github.io/development/2020/06/06/github-action/
* https://mong-blog.tistory.com/entry/CICD%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C-with-github-actions
* https://www.daleseo.com/github-actions-jobs/

## Code coverage

### Code coverage의 정의

Github Actions에서 각 steps마다 할일이 앞에서 언급했던 코드의 동작 확인이라면, 할일을 일일히 yml 파일에 입력해줄 수 있습니다.

하지만, 할 작업이 길 경우에는 yml가 너무 길어질 뿐더러 관리가 어렵기 때문에 불편할 수 있습니다.

이를 대체하기 위해 Code coverage 서비스를 사용할 수 있습니다.

_Code coverage_란 주어진 테스트에 대해 특정 소프트웨어의 코드가 얼마나 잘 실행되는지를 측정하는 것을 의미합니다.

크게 아래의 세 가지를 기준으로 "잘 실행"되는 정도를 측정합니다.

1. 구문(Statement): 단순히 얼마나 많은 줄(Line) 수의 코드가 실행되었는가. 
= (실행 줄 수 / 전체 줄 수)

2. 조건(Condition): 각 내부 조건이 가능한 참/거짓에 대해 실행되었는가. 
= ( True False가 모두 검증된 내부 조건 수 / 전체 내부 조건 수 )

3. 결정(Decision): 각 조건이 가능한 참/거짓에 대해 실행되었는가. 
= ( True False가 모두 검증된 조건 수 / 전체 조건 수 )

여기서 조건이란 여러 내부 조건의 집합을 의미하며, 아래와 같은 함수의 경우

`In[2]:`
```python
{% raw %}def test(a, b):
    if (a > 0 and b > 0):
        return "Two Positive"
    return "Otherwise"{% endraw %}
```

"a > 0" 및 "b > 0"는 내부 조건이고, (a > 0 and b > 0)는 조건이라고 할 수 있습니다.

즉, Code coverage는 Input에 따라 달라질 수 있으며 최대한 많은 Input을 고려하여 가능한 모든 오류를 대비하는 것에 목표가 있습니다.

예시로, (a, b)가 두 개의 입력[(1, -1), (-1, 1)]에 대해 실행되었다면, 아래와 같은 커버리지를 갖습니다.

1. 구문 커버리지: 조건문을 True로 통과하지 못하여 2줄이 실행되어, ( 2 / 4 ) = 50%.

2. 조건 커버리지: 내부 조건 a > 0에 대해서도 모두 검증되며, b > 0에 대해서도 모두 검증되었으므로 ( 4 / 4 ) = 100%.

3. 결정 커버리지: (a > 0 and b > 0)는 두 입력 모두 False만 출력되어 ( 1 / 2 ) = 50%.

더 많은 Code coverage의 정의는 아래를 참고 바랍니다.

* https://velog.io/@swhan9404/python-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%BB%A4%EB%B2%84%EB%A6%AC%EC%A7%80
* https://m.blog.naver.com/suresofttech/221833396343

### Github Actions과 Code coverage

업데이트된 코드에 대한 Coverage를 측정하는 것도 Github Actions를 통해 자동으로 진행할 수 있습니다.

가장 손쉽게는 _codecov_ 서비스를 활용하면 됩니다.

codecov는 각 Repository에서 측정된 coverage를 잘 정리해서 제공하고, 관리하는 서비스로 Github과 편리한 연동이 가능합니다.

사용하는 방법은 아래와 같습니다.

1. https://app.codecov.io/login/gh 에서 Github 연동

2. Coverage를 측정하고자 하는 Repository 내부에 test로 시작하거나 (test_*.py), test로 끝나는 (*_test.py) py 파일 생성.

3. yml에 codecov 코드 추가

2번 같은 경우 아래와 같이 구현 가능합니다.

`In[3]:`
```python
{% raw %}#test_import.py
import os
import sys
sys.path.insert(0, os.path.abspath(os.path.join(os.path.dirname(__file__), '..')))

def test_import():
    import mypackage
    assert True{% endraw %}
```

위 코드는 github repository의 가장 상위 폴더에 있는 mypackage라는 모듈을 불러오는 함수이며, 실행이 될 경우 100%가 되는 코드이고 아닐 경우 에러가 나게 됩니다.

pytest라는 모듈을 활용하면, test로 시작하거나 (test_*.py), test로 끝나는 (*_test.py) py 파일을 모두 찾아 내부의 (1) test로 시작하는 함수, (2) Test로 시작하는 클래스의 메소드를 전부 실행하여 Coverage를 측정합니다.

이를 Github action에 codecov에서 제공하는 아래 코드를 steps의 마지막에 삽입하면 자동으로 coverage를 볼 수 있습니다.

`In[4]:`
```ruby
{% raw %}# test.yml
name: test

on:
    ...
      - name: Run pytest
        run: |
          pip install pytest pytest-cov
          pytest --cov . --cov-report=xml
            
      - name: Upload coverage reports to Codecov
        run: |
          # Replace `linux` below with the appropriate OS
          # Options are `alpine`, `linux`, `macos`, `windows`
          curl -Os https://uploader.codecov.io/latest/linux/codecov
          chmod +x codecov
          ./codecov -t ${CODECOV_TOKEN}  # https://github.com/{github-id}/{repo-name}/settings/secrets/actions 에서 설정 가능.{% endraw %}
```

이 때, CODECOV_TOKEN은 codecov에서 만들 수 있으며, 자세한 내용은 아래를 참고하면 됩니다.

* https://docs.codecov.com/docs

## Tips

* Conda, PyTorch 역시 환경에서 세팅 가능합니다. 하지만, 안타깝게도 GPU는 지원이 되지 않아 이 경우 (1) CPU버전의 torch를 설치하거나, (2) self-hosted runner를 사용해야 합니다. (1)의 경우 GPU보다 처리 속도가 느리므로, 배치 수를 줄이는 것을 추천합니다.
    * https://libertegrace.tistory.com/entry/Github-CICD-Github-Actions-self-hosted-runner-with-own-GPUs
    * https://newsight.tistory.com/326
* Github 자체의 Issue를 라벨링하는 workflow는 아래를 참고하면 됩니다.
    * https://github.com/github/issue-labeler
