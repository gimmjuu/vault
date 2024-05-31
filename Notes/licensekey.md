---
tags:
  - LICENSE
---
# License Key Authentication

## 🎇 License Key Authentication Architecture

### 📌 Anonymous client에 대한 라이센스 발급

1. 클라이언트에서, CPU ID나 MAC 주소와 같은 signature를 이용하여 hash 값을 만들어서 서버에 라이센스 키를 요청
2. 서버에서는 이 내용을 바탕으로 새로운 키를 생성하고, 이 키를 private key로 encrypt하여 클라이언트에 보냄
3. 클라이언트는 이 메시지를 public key로 풀어서 text 내용은 미리 상호 약속한 내용 (예를 들어 MAC address, CPU ID 등)

➰　request에 클라이언트를 인증하기 위한 user id, password 등을 같이 실어 보낼 수 있다.

➰　라이센스 키 체크는 클라이언트에서 저장된 public key로 풀어서 일치 하지 않으면 다음 코드 부분으로 진행을 못 하도록 애플리케이션 처리를 해야 하는데, decompile 등을 통하여 뚫릴 수 있기 때문에, 코드 난독화 솔루션이 필수적이다.

➰　매번 키 인증을 받을 수 있지만 처음 Activation 된 후에는 local에 저장된 라이센스를 가지고 비교할 수도 있다. 이 라이센스는 client의 CPU ID 등과 binding 되기 때문에, 어차피 옮기더라도 사용이 불가능하다.

### 📌 Serial #를 이용한 방식

1. 서버에서 license key(with device hash) + Serial # 요구
2. 클라이언트는 Serial #를 확인하고 
3. License Key를 생성하고 Private Key와 함께 Encrypt
4. 서버는 Public Key로 Decrypt
5. 해독한 key를 cpu id, address 등과 함께 비교

➰　MS Windows와 동일한 방식이다.

➰　Serial #로 인증을 하고 라이센스를 내려주는 방식이고, 이미 사용된 Serial #는 disable 시킬 수 있다.

## 🎇 Get MAC address

### 📌 Python GETMAC

➰　get_mac_address 하나의 메소드만 제공하는 Python 외장 모듈

```
$ Python -m pip3 install getmac
```

```python
import getmac

getmac.get_mac_address()
```
`result` *'XX:XX:XX:XX:XX:XX'*

### 📌 Python psutil

```python
import psutil

nics = psutil.net_if_addrs()
print([j.address for j in nics[i] for i in nics if i!="lo" and j.family==17])
```

```python
nics = psutil.net_if_addrs()['eth0']

for interface in nics:
   if interface.family == 17:
      print(interface.address)
```

```python
import psutil

nics = psutil.net_if_addrs()
mac_address = nics['Ethernet'][0].address
print(mac_address)
```
`result` *네트워크 정보를 가진 Dictionary*

### 📌 Python uuid

```python
import uuid

t_node = uuid.getnode()
t_mac = ':'.join(("%012X" % node_)[i:i+2] for i in range(0, 12, 2))
```
