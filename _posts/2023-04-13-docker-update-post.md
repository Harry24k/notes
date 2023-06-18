---
layout: post
title: 도커 삭제 후 업데이트하기
tags: [Docker]
image:
  path: /images/abstract-1.jpg
  feature: abstract-1.jpg
---

이전 글 중에서 [도커를 활용해 서버를 구성하는 글](https://harry24k.github.io/deep-learning-server-docker-post/)이 있었는데, nvidia driver, Docker 및 nvidia-docker의 업데이트로 달라진 환경을 설정해보려 합니다.

## NVIDIA 드라이버 업그레이드

`In[1]:`
```ruby
{% raw %}sudo ubuntu-drivers autoinstall{% endraw %}
```

NVIDIA 드라이버가 업데이트되면, 재시작합니다.

`In[2]:`
```ruby
{% raw %}sudo reboot{% endraw %}
```

## 도커 삭제

기존 도커를 삭제하겠습니다.

`In[3]:`
```ruby
{% raw %}sudo apt-get purge -y docker-engine docker docker.io docker-ce docker-ce-cli nvidia-docker nvidia-docker2 nvidia-container-runtime{% endraw %}
```

`In[4]:`
```ruby
{% raw %}sudo apt-get autoremove -y --purge docker-engine docker docker.io docker-ce nvidia-docker nvidia-docker2 nvidia-container-runtime{% endraw %}
```

`In[5]:`
```ruby
{% raw %}sudo apt-get remove docker docker-engine docker.io containerd runc{% endraw %}
```

`In[6]:`
```ruby
{% raw %}sudo rm -rf /var/lib/docker /etc/docker{% endraw %}
```

`In[7]:`
```ruby
{% raw %}sudo rm /etc/apparmor.d/docker{% endraw %}
```

`In[8]:`
```ruby
{% raw %}sudo groupdel docker{% endraw %}
```

`In[9]:`
```ruby
{% raw %}sudo rm -rf /var/run/docker.sock{% endraw %}
```

## 도커 설치하기

이제 새로 도커를 설치해야 합니다.

#### Ubuntu 18.04 이상

`In[10]:`
```ruby
{% raw %}sudo apt update{% endraw %}
```

`In[11]:`
```ruby
{% raw %}sudo apt-get install \
    ca-certificates \
    curl \
    gnupg{% endraw %}
```

`In[12]:`
```ruby
{% raw %}sudo mkdir -m 0755 -p /etc/apt/keyrings{% endraw %}
```

`In[13]:`
```ruby
{% raw %}curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg{% endraw %}
```

`In[14]:`
```ruby
{% raw %}echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null{% endraw %}
```

`In[15]:`
```ruby
{% raw %}sudo apt update{% endraw %}
```

`In[16]:`
```ruby
{% raw %}sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin{% endraw %}
```

`In[17]:`
```ruby
{% raw %}docker --version{% endraw %}
```

#### Ubuntu 16.04

`In[18]:`
```ruby
{% raw %}sudo apt update{% endraw %}
```

`In[19]:`
```ruby
{% raw %}sudo apt-get install -y ca-certificates curl software-properties-common apt-transport-https gnupg lsb-release{% endraw %}
```

`In[20]:`
```ruby
{% raw %}curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
sudo apt update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io {% endraw %}
```

`In[21]:`
```ruby
{% raw %}docker --version{% endraw %}
```

아래는 nvidia-docker 설치 방법입니다.

`In[22]:`
```ruby
{% raw %}curl -s -L https://nvidia.github.io/nvidia-container-runtime/gpgkey | \
  sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-container-runtime/$distribution/nvidia-container-runtime.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-container-runtime.list
sudo apt-get update{% endraw %}
```

`In[23]:`
```ruby
{% raw %}apt-get install nvidia-container-runtime{% endraw %}
```

`In[24]:`
```ruby
{% raw %}which nvidia-container-runtime-hook{% endraw %}
```

`In[25]:`
```python
{% raw %}s#[CodeIn]ruby
{% raw %}sudo apt-get install -y nvidia-docker2{% endraw %}
```

`In[26]:`
```ruby
{% raw %}sudo systemctl restart docker{% endraw %}
```

## 확인하기

`In[27]:`
```ruby
{% raw %}docker run -it --rm --gpus all ubuntu nvidia-smi{% endraw %}
```

## [별첨] Jupyter Notebook 세팅하기

`In[28]:`
```ruby
{% raw %}jupyter notebook --generate-config{% endraw %}
```

`In[29]:`
```ruby
{% raw %}vim /workspace/.jupyter/jupyter_notebook_config.py{% endraw %}
```

`In[30]:`
```ruby
{% raw %}export JUPYTER_CONFIG_DIR=/workspace/.jupyter/{% endraw %}
```

`In[31]:`
```ruby
{% raw %}chmod 775 -R workspace{% endraw %}
```

`In[32]:`
```ruby
{% raw %}sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A OUTPUT -o lo -j ACCEPT{% endraw %}
```

`In[33]:`
```ruby
{% raw %}jupyter notebook --ip 0.0.0.0 --no-browser --allow-root --port 8890{% endraw %}
```
