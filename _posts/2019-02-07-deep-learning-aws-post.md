---
layout: post
title: 딥러닝을 위한 AWS 세팅하기
tags: [Deep Learning]
image:
  path: /images/abstract-1.jpg
  feature: abstract-1.jpg
comments: true
---

이번 글에서는 딥러닝을 사용하기 위한 AWS 서버 세팅을 다루어 보겠습니다.

**참고 자료 :**
> [https://supdev.tistory.com/22](https://supdev.tistory.com/22)

> [https://arisu1000.tistory.com/27557](https://arisu1000.tistory.com/27557)

> [http://blog.impiyush.com/2015/02/running-ipython-notebook-server-on-aws.html](http://blog.impiyush.com/2015/02/running-ipython-notebook-server-on-aws.html)

> [https://medium.com/@ansxownszz/jupyter-notebook-auto-start-on-boot-deep-learning-ami-ec2-instance-c1b92d8de030](https://medium.com/@ansxownszz/jupyter-notebook-auto-start-on-boot-deep-learning-ami-ec2-instance-c1b92d8de030)

- - -

## 1. AWS 가입 및 설정

### - AWS 가입

아래 주소를 통해 AWS에 가입합니다.

[https://portal.aws.amazon.com/billing/signup?redirect_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation&language=ko_kr#/start](https://portal.aws.amazon.com/billing/signup?redirect_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation&language=ko_kr#/start)

### - EC2 등록하기

![EC2]({{site.url}}/images/deep-learning-aws-post/EC2.png){:.center-image}

**EC2(인스턴스)란?** Amazon Elastic Compute Cloud(EC2)는 안전하고 크기 조정이 가능한 컴퓨팅 파워를 클라우드에서 제공하는 웹 서비스입니다. 개발자가 더 쉽게 웹 규모의 클라우드 컴퓨팅 작업을 할 수 있도록 설계되었습니다.

[https://aws.amazon.com/ko/ec2](https://aws.amazon.com/ko/ec2)

위 주소로 접속하여 "Amazon EC2 시작하기"를 클릭합니다.

![Start Instance]({{site.url}}/images/deep-learning-aws-post/Start Instance.png){:.center-image}

그러면 AWS 관리 화면으로 이동되고, EC2 대시보드 탭에서 "인스턴스 시작"을 누르면 인스턴스 생성으로 진행됩니다.

![Select AMI]({{site.url}}/images/deep-learning-aws-post/Select AMI.png){:.center-image}

AWS에서는 다양한 Machine Image를 제공하고 있으며, Deep Learning에서 사용하는 보편적인 세팅은 Ubuntu/Linux입니다.

따라서, Ubuntu Server 혹은 Amazon Linux를 사용하게 되는데, 본문에서는 Deep Learning 학습에 필요한 여러 프레임워크(TensorFlow, PyTorch 등)이 이미 모두 준비 되어 있는 **"Deep Learning AMI (Ubuntu) Version 20.0"**을 사용하겠습니다.

"Deep Learning AMI (Ubuntu) Version 20.0"의 경우 Anaconda가 기본적으로 설치되어 있어, 다른 설치가 필요없는 것이 장점입니다.

![Select Type]({{site.url}}/images/deep-learning-aws-post/Select Type.png){:.center-image}

이제 Instance Type을 설정해야합니다.

프리티어(12개월 무료)로 사용하고 싶으신 분들은 **"프리 티어 사용 가능"**을 누르시면 되지만, 이 경우 GPU는 제공이 되지 않습니다.

따라서 GPU를 활용한 Deep Learning을 하고자 하시는 분들은 유형이 "GPU instances"인 것들 중에 사용하시면 됩니다.

단, 이 경우 비용이 많이 들 수 있으니 아래 사이트를 참고 바랍니다. 참고로 현재 기준 p2.xlarge는 시간당 1,000원 정도입니다.

[https://aws.amazon.com/ko/ec2/pricing/on-demand/](https://aws.amazon.com/ko/ec2/pricing/on-demand/)

이 후 스토리지 설정 등 처음 AWS를 사용하시는 분이라면 다양한 설정이 나오게 되는 데, 이는 각자의 생각에 맞게끔 설정하시면 됩니다.

만약 딱히 원하는 설정을 생각하지 않으신 경우 계속 다음을 클릭하여 단계 7까지 진행해주시면 됩니다.

![Review]({{site.url}}/images/deep-learning-aws-post/Review.png){:.center-image}

단계 7까지 진행하셨다면, 해당 EC2에 대한 설정이 요약되어 등장하며, 내용이 맞다면 우측 하단의 "시작"을 누르고 다음으로 넘어갑니다.

![Key Pair]({{site.url}}/images/deep-learning-aws-post/Key Pair.png){:.center-image}

그러면 마지막으로 키 페어를 설정하는 부분이 나옵니다.

만들어 놓은 서버에 접속하기 위해서는 이 키 페어가 반드시 필요하며,

처음 하시는 경우 "새 키 페어 생성"을 클릭하고 이름을 입력하여 키 페어를 생성합니다.

그 후 "키 페어 다운로드"를 클릭하여 **잘 보관**한 다음 "인스턴스 시작"을 눌러 EC2를 실행합니다,

## 2. EC2와 연결하기 (Windows)

![Instance Page]({{site.url}}/images/deep-learning-aws-post/Instance Page.png){:.center-image}

바로 볼 수 있으며, 이는 "AWS Management Console"에서 보실 수 있습니다.

이제 남은 건 실행되고 있는 EC와 연결하는 것입니다.

혹시 인스턴스가 위 그림처럼 인스턴스 상태가 "stopped"로 꺼져있다면, 해당 인스턴스를 체크하신 후 아래와 같이 해주시면 됩니다.

* 작업 > 인스턴스 상태 > 시작
* 주의 1 : 인스턴스를 종료할 때는 "중지" 버튼을 통해 멈춥니다. "종료" 버튼을 누르면 인스턴스가 삭제됩니다.
* 주의 2 : 인스턴스가 켜져있는 동안은 돈을 소모하게 됩니다. 사용하지 않을 땐 "중지"를 눌러 꺼놓도록 합니다.

### - Putty 설치

Putty는 서버를 원격으로 제어하기 위한 프로그램이며, 무료 오픈소스 프로그램입니다.

아래 링크에서 다운받을 수 있으며, 설치 방법은 간단하기 때문에 생략하도록 하겠습니다.

[https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

우선 설치가 끝났다면, 설치 폴더 혹은 윈도우 메뉴에서 "PuTTYgen"이라는 것을 찾아 실행합니다.

![PuTTYgen]({{site.url}}/images/deep-learning-aws-post/puttygen.png){:.center-image}

여기서 Load를 클릭 후 위에서 보관했던 키 페어 파일(.pem)을 불러옵니다.

![PuTTYgen Tip]({{site.url}}/images/deep-learning-aws-post/puttygentip.png){:.center-image}

단, 이 때 키 페어 파일은 .pem이므로 위와 같이 파일 확장자를 All files로 바꿔주셔야 합니다.

![PuTTYgen Notice]({{site.url}}/images/deep-learning-aws-post/puttygennotice.png){:.center-image}

제대로 불러와졌으면 PuTTYgen Notice라는 창이 뜨는데, 확인을 눌러줍니다. 

그러면 암호화할 구문을 입력하지 않았다는 Warning이 뜨는데 가볍게 예를 선택하여 private key(.ppk)를 저장합니다.

이제 "PuTTYgen"이라는 것을 끄고, "PuTTY"를 실행합니다.

![PuTTY.png]({{site.url}}/images/deep-learning-aws-post/putty.png){:.center-image}

"PuTTY" 프로그램이 실행되면, Connection - SSH - Auth로 이동하여 Browse를 눌러 아까 저장했던 PuTTY key 파일을 불러옵니다.

![Auth and Browse.png]({{site.url}}/images/deep-learning-aws-post/AutoBrowse.png){:.center-image}

그 후, Seession으로 이동합니다.

이제 Session에 "Host Name"을 입력해줄 차례입니다.

![Publice DNS.png]({{site.url}}/images/deep-learning-aws-post/Publice DNS.png){:.center-image}

"Host Name"은 현재 실행 중인 AWS 인스턴스 설명의 "퍼블릭 DNS(IPv4)"에 해당됩니다.

위의 경우에는 **"ec2-3-16-155-150.us-east-2.compute.amazonaws.com"**이 되겠습니다.

![puttyalert.png]({{site.url}}/images/deep-learning-aws-post/puttyalert.png){:.center-image}

복사하여 입력 후, Open 버튼을 클릭하면 경고창이 나오는데, 이 역시 무시 후 예를 클릭하면 처음 서버에 접속하는 지금의 PuTTY key가 등록됩니다. 앞으로는 이 key를 활용하여 인스턴스에 접속할 수 있습니다.

그럼 이제 EC2 인스턴스에 접속하였습니다!

![login.png]({{site.url}}/images/deep-learning-aws-post/login.png){:.center-image}

### - EC2 로그인 및 Jupyter Notebook 확인

로그인을 하기 위해서는 우선 ubuntu라는 계정으로 로그인을 해야합니다.

"ubuntu"라고 입력한 뒤 엔터를 입력해주세요

![ubuntu.png]({{site.url}}/images/deep-learning-aws-post/ubuntu.png){:.center-image}

로그인이 성공적으로 이루어지면 다음과 같은 화면이 뜨게 됩니다.

![ubuntulogin.png]({{site.url}}/images/deep-learning-aws-post/ubuntulogin.png){:.center-image}

앞서 말씀드린 것과 같이, 저희가 설치한 이 EC2에는 이미 딥러닝을 위한 전반적인 설정이 다 되어 있습니다.

따라서, 여기에는 Anaconda가 이미 깔려져 있으며 Jupyter Notebook 역시 실행 가능합니다!

빈칸에 "jupyter notebook"이라고 입력 후 엔터를 입력하시면

![jupyter notebook.png]({{site.url}}/images/deep-learning-aws-post/jupyter notebook.png){:.center-image}

다음과 같이 Jupyter Notebook이 실행됩니다.

하지만 지금은 밖에서 접근할 수가 없습니다. 왜냐하면 아직 외부에서 접근할 수 있도록 세팅하지 않았기 때문이죠.

## 3. Jupyter Notebook 외부 접속 허용

### - 보안 그룹 설정

우선, EC2에서의 Jupyter Notebook을 접속하기 위해서는 EC2 자체의 보안 설정을 바꿔주셔야합니다.

다시 EC2 모듈을 설정하는 AWS 홈페이지로 돌아갑니다.

![Publice DNS.png]({{site.url}}/images/deep-learning-aws-post/Publice DNS.png){:.center-image}

그 후 왼쪽에 있는 EC2 대시보드 아래의 "네트워크 및 보안 > 보안 그룹"으로 들어갑니다.

그러면 여태까지 생성했던 혹은 하나의 보안 설정들이 있는데 이제 저희는 새로운 하나를 추가할 계획입니다.

"보안 그룹 생성"을 클릭하여 새로운 보안 규칙을 만들겠습니다.

![secure.png]({{site.url}}/images/deep-learning-aws-post/secure.png){:.center-image}

위에서 "Sample"이라는 이름만 다르고 나머지는 모두 똑같이 만들어주시면 됩니다.

규칙이 하나일 경우에는 "규칙 추가"를 눌러 그림처럼 세 개로 늘릴 수 있습니다.

그 후 "생성" 버튼을 눌러 새로운 보안 규칙을 적용할 수 있습니다.

위 과정이 완료되었다면, 다시 인스턴스 페이지로 돌아갑니다.

그리고 해당 EC2 모듈을 오른쪽 클릭한 후 "네트워킹 > 보안 그룹 변경"을 클릭해주세요.

![networking.png]({{site.url}}/images/deep-learning-aws-post/networking.png){:.center-image}

그러면 보안 그룹을 변경할 수 있게되는데, 이 때 아까 생성했던 보안 그룹으로 선택해주신 후 "보안 그룹 할당"을 눌러주시면 됩니다.

### - Jupyter Notebook 설정

다시 아까의 PuTTY로 돌아와서, 아까 실행시켰던 Jupyter Notebook을 Ctrl+C를 엔터를 눌러 종료하도록 합니다.

그러면 "Shutdown this notebook server"를 물어보는데, y를 입력한 후 엔터를 눌러 완벽히 종료해줍니다.

![ctrlc.png]({{site.url}}/images/deep-learning-aws-post/ctrlc.png){:.center-image}

이제부터는 아래 명령어를 입력해주시면 됩니다.


```python
python
> from IPython.lib import passwd
> passwd()
```

![passwd.png]({{site.url}}/images/deep-learning-aws-post/passwd.png){:.center-image}

그러면 위와 같이 password를 입력하라는 창이 뜨게 됩니다.

이 때 주의할 점은 password를 보이지는 않지만 입력이 되고 있기 때문에 한 번에 입력하고자 하는 비밀번호를 입력하는 것이 좋습니다.

비밀번호를 입력하면 한 번 더 입력하라는 "Verify password"라는 문구가 뜨게 됩니다.

두 비밀번호가 일치했다면, 아래에 **'sha~'**로 시작하는 문구가 나오는 데 이 문장을 잘 저장하고 아래 명령어를 입력하여 python을 빠져나옵니다.


```python
> exit()
```

그 후 아래의 명령어를 입력합니다.


```python
jupyter notebook --generate-config
mkdir certs
cd certs
sudo openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.key -out mycert.pem
```

위 과정을 진행하면 몇 가지 질문(국가/도시/회사 등)을 물어보는 데 적절히 답하거나 마침표를 입력한 후 엔터를 눌러 넘어가도록 합니다.

모두 완료가 되었다면 아래의 명령어를 추가로 실행합니다.


```python
cd ~/.jupyter/
```

이제 ubuntu에서 제공하는 파일 편집기 vi를 통해 변경을 해보겠습니다.

* 파일 생성 혹은 편집 :


```python
vi jupyter_notebook_config.py
```

* 저장 후 종료 :


```python
:wq 
```

이 외의 vi 사용법은 본문에서 다루기는 길기 때문에 구글을 통해 검색하거나, 어려우신 분들은 본문 하단에 [Filezila]를 설치하신 후 변경하시는 것을 권장드립니다.

**주의할 점으로 원래 파일에는 아래 문장들 앞에 #이 있을겁니다. 이는 주석을 의미하기 때문에 꼭 지워주셔야 반영이 됩니다.**

* c.NotebookApp.certfile = u'/home/ubuntu/certs/mycert.pem'

* c.NotebookApp.keyfile = u'/home/ubuntu/certs/mycert.key'

* c.NotebookApp.ip = '\*'

* c.NotebookApp.open_browser = False

* c.NotebookApp.port = 8888


추가적으로 아래도 바꾸어 주셔야 합니다.

* c.NotebookApp.password = u'sha~'

위의 'sha~'가 바로 위에서 저장해두었던 **'sha~'**입니다. 앞의 u는 남긴 후에 똑같이 써주시면 됩니다.

참고로 Jupyter Notebook 시작 폴더를 변경하고 싶으신 분들은 아래를 변경해주시면 됩니다.

* c.NotebookApp.notebook_dir = ''

이제 Jupyter notebook을 입력하여 아까와 똑같이 실행합니다.

![jupyter notebook.png]({{site.url}}/images/deep-learning-aws-post/jupyter notebook.png){:.center-image}

그러나 이제 우리는 컴퓨터의 인터넷 브라우저로 Jupyter Notebook에 접속할 수 있습니다!

인터넷 브라우저에 **https://"퍼블릭 DNS(IPv4)":8888** 이라고 입력해주시면 됩니다.

예를 들어, "퍼블릭 DNS(IPv4)"가 "ec2-3-16-155-150.us-east-2.compute.amazonaws.com"이라면 아래와 같이 인터넷 브라우저에 입력해주시면 됩니다.

* https://ec2-3-16-155-150.us-east-2.compute.amazonaws.com:8888

이제 여러분은 EC2를 통해 Jupyter Notebook을 돌릴 수 있게 되었습니다!

## 4. Filezila를 통한 파일 이동

안타깝게도 Jupyter Notebook으로 파일을 옮기는 것은 제한적입니다.

따라서 다른 프로그램을 통해 인스턴스의 파일을 관리할 필요가 있습니다.

우선 아래 사이트에서 "FileZilla Client"를 다운받습니다.

[http://filezilla-project.org/](http://filezilla-project.org/)

설치는 생략하도록 하겠습니다.

![filezilla.png]({{site.url}}/images/deep-learning-aws-post/filezilla.png){:.center-image}

설치를 마치면 FileZilla를 실행할 수 있고, 실행하면 위와 같은 화면이 뜨게 됩니다.

여기서 우선 "편집 > 설정"을 눌러줍니다.

![filezillakey.png]({{site.url}}/images/deep-learning-aws-post/filezillakey.png){:.center-image}

그럼 위와 같은 화면이 나오는 데, 이 때 "키 파일 추가"를 눌러 위에서 EC2에 등록했던 '.ppk'로 끝나는 파일을 추가해줍니다.

단, 이 때 .ppk의 경로 이름에는 한글이 포함되지 않아야합니다.

그 후에는 확인을 누른 후에, 다시 원래 화면으로 돌아가서 "파일 > 사이트 관리자"를 클릭해줍니다. (혹은 Ctrl + S)

![siteadmin.png]({{site.url}}/images/deep-learning-aws-post/siteadmin.png){:.center-image}

그럼 위와 같이 창이 뜨게 되는데, 이 때 아래와 같이 입력해주시면 됩니다.

* 호스트 :"퍼블릭 DNS(IPv4)"
* 포트 : 22
* 프로토콜 : SFTP
* 로그온 유형 : 일반
* 사용자 : ubuntu

물론 "퍼블릭 DNS(IPv4)"은 ec2-3-16-155-150.us-east-2.compute.amazonaws.com와 같은 주소로 바꾸어 써주셔야 합니다.

그리고 "연결"을 누르면 이제 여러분은 EC2의 파일 시스템에 접속 가능하게 됩니다!

![connect.png]({{site.url}}/images/deep-learning-aws-post/connect.png){:.center-image}

위가 바로 연결된 이미지이며, 오른쪽 "리모트 사이트"가 현재 EC2의 파일 시스템을 나타내며, 왼쪽 "로컬 사이트"가 현재 컴퓨터의 파일 시스템을 나타냅니다.

여러분은 이제 오른쪽 파일을 왼쪽으로 "드래그&드랍"하거나 반대를 통해 파일을 주고 받을 수 있습니다.

이 외의 자세한 FileZilla 사용법은 구글을 통해 참고 바랍니다.

## 5. 인스턴스 시작시 자동으로 Jupyter Notebook 실행

하지만, 인스턴스를 켤 때마다 PuTTY에 접속해서 Jupyter Notebook을 실행하기에는 시간도, 비용도 들게 됩니다.

따라서, 여기서는 인스턴스를 부팅할 때 자동으로 Jupyter Notebook을 실행하는 방법을 알아보겠습니다.

우선 **/home/ubuntu/.jupyter** 폴더에 start_notebook.sh라는 파일을 만듭니다.

위의 파일 안에는 아래와 같은 내용이 들어가야 합니다.

* #!/bin/bash
* export PATH="/home/ubuntu/anaconda3/bin:$PATH"
* jupyter notebook &

이는 vi를 활용하거나, 메모장으로 start_notebook.sh를 만든 후 FileZilla로 이동해도 상관없습니다.

그 후 아래 명령어를 통해 etc 폴더의 rc.local 파일을 수정합니다.


```python
sudo nano /etc/rc.local
```

이는 관리자 권한으로 인해 FileZila는 사용할 수 없으며, nano를 통해 수정하는 것을 권장드립니다. nano의 자세한 사용법은 아래와 같습니다.

* 방향키 : 이동
* 키보드 : 입력
* Ctrl + o : 현재 파일 저장
* Ctrl + x : 편집 종료

위 사용법만 알아도 아래를 수행하는 데에는 문제 없습니다.

rc.local 파일에는 기본적으로 **exit 0**이라는 문장이 있는 이미 상태일 것입니다.

그 상태에서 **exit 0** 위에 아래 문구를 입력해줍니다.

* cd /home/ubuntu
* su ubuntu -c 'bash /home/ubuntu/.jupyter/start_notebook.sh'

그 후에 저장을 한 후 편집 종료를 해줍니다.

이로써 여러분은 EC2를 시작할 때마다 PuTTY를 통해 접속할 필요없이, 자동적으로 Jupyter Notebook을 킬 수 있게 되었습니다!

앞으로 EC2를 시작하신 후, https://"퍼블릭 DNS(IPv4)":8888 페이지로 접속하시면 편하게 Jupyter Notebook을 만나실 수 있게될 것입니다.