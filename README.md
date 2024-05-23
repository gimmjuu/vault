# Vault

> obsidian-sync
---
### MarkDown common

##### 1. 제목 ==*headers*==

> [! note] `#` 개수에 따라 제목 수준을 결정

- # 제목 1 : `# 제목 1`
- ## 제목 2 : `## 제목 2`
- ### 제목 3 : `### 제목 3`
- #### 제목 4 : `#### 제목 4`
- ##### 제목 5 : `##### 제목 5`
- ###### 제목 6 : `###### 제목 6`

##### 2. 굵게 ==*bold*==

> [!note] `**단어**` 또는 `__단어__`

- **단어** : `**단어**`
- __단어__ : `__단어__`

##### 3. 기울임 ==*italic*==

> [!note] `*단어*` 또는 `_단어`

- *단어* : `*단어*`
- _단어_ : `_단어_`

##### 4. 취소선 ==*strilethrough*==

> [!note] `~~단어~~`

- ~~단어~~ : `~~단어~~`

##### 5. 목록 ==*list*==

>[!note] 순서가 있는 리스트 `1.`, `2.`, `3.`, ...

1. `1.`
2. `2.`
3. `3.`

> [!note] 순서가 없는 리스트 `-`, `+`, `*`

- `-`
+ `+`
* `*`

##### 6. 링크 ==*link*==

> [!note] `[링크 텍스트](URL)` 링크 텍스트를 클릭하면 지정 URL로 연결

- [Google](https://www.google.co.kr/) : `[Google](https://www.google.co.kr/)`

##### 7. 이미지 ==*image*==

>[!note] `![대체 텍스트](이미지 URL)`

 ![위키피디아 흑요석](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/ObsidianOregon.jpg/360px-ObsidianOregon.jpg) 
- `![위키피디아 흑요석](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/ObsidianOregon.jpg/360px-ObsidianOregon.jpg)`

##### 8. 인용구 ==*blockquote*==

> [!note] `>` 인용구 블록

> [!note] `/` 이스케이프 문자

##### 9. 코드 ==*code*==

>[!note] \`인라인 코드\` 또는 \`\`\`코드 블록\`\`\`

- `인라인 코드` : \`인라인 코드\`
- ```코드 블록``` : \`\`\`코드 블록\`\`\`

##### 10. 수평선 ==*horizontal rule*==

> [!note] `---`, `___`, `***`

---
- `---`
___
- `___`
***
- `***`

##### 11. 각주 ==*footnote*==

> [!note] 기본 각주 `[^1]`

- 기본 각주[^1] : `기본 각주[^1]`
[^1]: 각주 참조 : `[^1]: 각주 참조`

> [!note] 여러 줄 각주 `[^2]: \n _ _` 각주 참조 새 줄 앞 공백 2개

- 여러 줄 각주[^2] : `여러 줄 각주[^2]`
[^2]: 여러 줄 각주 참조.
  각주 참조

> [!note] 이름 각주 `[^name]`

- 이름 각주[^name] : `이름 각주[^name]`
[^name]: 이름 각주 참조

> [!note] 인라인 각주 `^[인라인 각주]`

- 인라인 각주 ^[각주 참조]

---

### Obsidian only

##### 1. 하이라이트 ==*highlight*==

>[!note] `==단어==`

- ==단어== : `==단어==`

##### 2. 체크박스 ==*checkbox*==

> [!note] `- [ ]` 기본 체크 박스 (단축키 `Ctrl + L`)

- [ ] 기본 체크 박스 `- [ ]`

> [!note] `- [-]` 완료 체크 박스

- [-] 완료 체크 박스 `- [-]`

> [!note] `- [x]` 취소 체크 박스

- [x] 취소 체크 박스 `- [x]`

##### 3. 그림 크기 ==*image size*==

> [!note] `![대체 텍스트|가로x세로](이미지 URL)`

![위키피디아 흑요석|100x100](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/ObsidianOregon.jpg/360px-ObsidianOregon.jpg)
- `![위키피디아 흑요석|100x100](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/ObsidianOregon.jpg/360px-ObsidianOregon.jpg)`

##### 4. 문서 링크 ==*internal link*==

> [!note] `[[노트 또는 블록 이름]]` 노트와 노트 또는 블록과 블록을 서로 링크

- [[README]] : `[[README]]`

##### 5. 문서 임베딩 ==*embedding link*==

>[!note] `![[노트 이름]]` 노트 내용 자체를 다른 노트에 임베드
 
 - ![[Template]] : `![[Template]]`

##### 6. 문단 참조 ==*block reference*==

> [!note] `![[블록 링크 #^id]]` 다른 블록 내용을 현재 블록에 참조

- ![[Template#^sampleblock]] : `![[Template#^sampleblock]]`

##### 7. 강조 상자 ==*callout*==

> [!note] : `> [!note]` 콜아웃 기본 아이콘

> [!abstract] : `> [!abstract]`

> [!summary] : `> [!summary]`

> [!tldr] : `> [!tldr]`

> [!info] : `> [!info]`

> [!todo] : `> [!todo]`

> [!tip] : `> [!tip]`

> [!hint] : `> [!hint]`

> [!important] : `> [!important]`

> [!success] : `> [!success]`

> [!check] : `> [!check]`

> [!done] : `> [!done]`

> [!question] : `> [!question]`

> [!help] : `> [!help]`

> [!faq] : `> [!faq]`

> [!warning] : `> [!warning]`

> [!caution] : `> [!caution]`

> [!attention] : `> [!attention]`

> [!failure] : `> [!failure]`

> [!fail] : `> [!fail]`

> [!missing] : `> [!missing]`

> [!danger] : `> [!danger]`

> [!error] : `> [!error]`

> [!bug] : `> [!bug]`

> [!example] : `> [!example]`

> [!quote] : `> [!quote]`

> [!cite] : `> [!cite]`
