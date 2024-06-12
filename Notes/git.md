---
tags:
  - DEVELOP
  - GIT
---

# Git

## Config
```bash
```
### Global
#### 설정 확인하기
```bash
git config --list 

git config --global --list
```

#### 설정 정보 등록하기
- 💥 config 등록 시 오타까지 그대로 등록되기 때문에 등록 후 list 확인 필요! (저도 알고 싶지 않았어요...)

```bash
git config user.name "user 이름"
git config user.email "user 이메일"
git config user.password "user token"

git config --global user.name "user 이름"
git config --global user.email "user 이메일"
git config --global user.password "user token"
```

#### 설정 정보 삭제하기
```bash
git config --unset user.name
git config --unset user.email
git config --unset user.password

git config --unset --global user.name
git config --unset --global user.email
git config --unset --global user.password
```

## .gitignore
- 쉘로 추가하기
```bash
echo __pycache__/ >> .gitignore
```