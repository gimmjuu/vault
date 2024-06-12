# Overview

*obsidian-sync*


# 🎇MarkDown common
## <span style="font-weight:600; font-style: italic;">Headers</span>
> `#` 개수에 따라 제목 수준을 결정

    # 제목 1
    ## 제목 2
    ### 제목 3
    #### 제목 4
    ##### 제목 5
    ###### 제목 6

## <span style="font-weight:600; font-style: italic;">Bold</span>
**굵게**

    **굵게**

__굵게__

    __굵게__

## <span style="font-weight:600; font-style: italic;">Italic</span>
*기울임*

    *기울임*

_기울임_

    _기울임_

<span style="font-style:italic;">기울임</span>

    <span style="font-style:italic;">기울임</span>

## ✔ <span style="font-weight:600; font-style: italic;">Font Color</span>
<span style="color:red">Text</span>
    
    <span style="color:red">Text</span>

## ✔ <span style="font-weight:600; font-style: italic;">BG Color</span>
<span style="background:rgba(255,0,0,0.25)">Text</span>

    <span style="background:rgba(255,0,0,0.25)">Text</span>

## <span style="font-weight:600; font-style: italic;">Strilethrough</span>
~~단어~~

    ~~단어~~

## <span style="font-weight:600; font-style: italic;">List</span>
### Ordered List
> 순서가 있는 리스트

1. `1.`
2. `2.`
3. `3.`

### Unordered List
> 순서가 없는 리스트

- `-`
+ `+`
* `*`


## ✔ <span style="font-weight:600; font-style: italic;">Table</span>

|📌|제목|소제목|내용|빈 줄|
|:---:|:---|:---:|---:|:---:|
||왼쪽 정렬||||
|||가운데 정렬|||
||||오른쪽 정렬||


    |📌|제목|소제목|내용|빈 줄|
    |:---:|:---|:---:|---:|:---:|
    ||왼쪽 정렬||||
    |||가운데 정렬|||
    ||||오른쪽 정렬||

## <span style="font-weight:600; font-style: italic;">Link</span>
> [링크 텍스트](URL) 링크 텍스트를 클릭하면 지정 URL로 연결

[Google](https://www.google.co.kr/)

    [Google](https://www.google.co.kr/)


## ✔ <span style="font-weight:600; font-style: italic;">Image</span>
### 🔍 이미지 리사이즈 불가
> `![대체 텍스트](이미지 URL)`

![위키피디아 흑요석](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/ObsidianOregon.jpg/360px-ObsidianOregon.jpg)

    ![위키피디아 흑요석](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/ObsidianOregon.jpg/360px-ObsidianOregon.jpg)

> `![대체 텍스트][참조 텍스트]` & `[참조 텍스트]: 이미지 URL`

![위키피디아 흑요석][흑요석 이미지]

[흑요석 이미지]: https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/ObsidianOregon.jpg/360px-ObsidianOregon.jpg

    ![위키피디아 흑요석][흑요석 이미지]

    [흑요석 이미지]: https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/ObsidianOregon.jpg/360px-ObsidianOregon.jpg


### ⚡ 이미지 리사이즈
> `<img src="이미지 URL" alt="대체 텍스트" width="너비" height="높이"/>`


<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/ObsidianOregon.jpg/360px-ObsidianOregon.jpg" width="100" height="100" alt="위키피디아 흑요석"/>

    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/ObsidianOregon.jpg/360px-ObsidianOregon.jpg" width="100" height="100" alt="위키피디아 흑요석"/>

> $cf.$ 이미지 중앙 정렬

<center><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/ObsidianOregon.jpg/360px-ObsidianOregon.jpg" width="100" height="100" alt="위키피디아 흑요석"/></center>

    <center><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/ObsidianOregon.jpg/360px-ObsidianOregon.jpg" width="100" height="100" alt="위키피디아 흑요석"/></center>


## <span style="font-weight:600; font-style: italic;">Blockquote</span>

> 인용구 블록

    > 인용구 블록

$cf.$ `\` 인용구 탈출 문자(이스케이프 문자)

> \`이스케이프 문자 사용\` vs `이스케이프 문자 미사용`

## ✔ <span style="font-weight:600; font-style: italic;">Code</span>
`인라인 코드`

    `인라인 코드`

```
    ```
    코드 블록
    ```
```

### $cf.$ 코드블록 식별 가능 언어 🔥
[내용 참조](https://computer-science-student.tistory.com/366)

||언어|태그||
|:---:|:---:|:---:|:---:|
||Bash|bash||
||C#|cs||
||C++|cpp||
||CSS|css||
||Diff|diff||
||HTML or XML|html||
||HTTP|http||
||Ini|ini||
||JSON|json||
||Java|java||
||JavaScript|javascript||
||PHP|php||
||Perl|perl||
||Python|python||
||Ruby|ruby||
||SQL|sql||
||`직접 ↓`|`추가 +`||
||TOML|toml||
||R|r||
||...|...||
<br>

## <span style="font-weight:600; font-style: italic;">Horizontal Rule</span>
---
    --- dash
___
    ___ under score
***
    *** asterick

## ✔ <span style="font-weight:600; font-style: italic;">Collapsible Sections</span>
<details>
    <summary style="padding-bottom:10px;">콜랩스 타이틀</summary>
    콜랩스(섹션을 확장하거나 축소) 내용
</details>
<br>

    <details>
        <summary style="padding-bottom:10px;">콜랩스 타이틀</summary>
        콜랩스 내용
    </details>

## ✔ <span style="font-weight:600; font-style: italic;">Equation</span>

|기호|사용|뜻|
|:-:|:-:|:-|
|$|$f = ma$|기본 수식 기호|
|^|$2^{i+1}$|위 첨자|
|_|$2_{i-1}$|아래 첨자|
|\frac|$\frac{1}{2}$|분수|
|\sqrt|$\sqrt{d}$|루트|

## <span style="font-weight:600; font-style: italic;">Footnote</span>

이것은 각주를 사용한 문장입니다.[^1]

[^1]: 각주 내용입니다. 여기에 추가적인 설명이나 참고 자료를 작성할 수 있습니다.

여러 줄 각주 `[^2]: \n _ _` 각주 참조 새 줄 앞 공백 2개

    여러 줄 각주[^2] : `여러 줄 각주[^2]`
[^2]: 여러 줄 각주 참조.
    각주 참조

이름 각주 `[^name]`

    이름 각주[^name] : `이름 각주[^name]`
[^name]: 이름 각주 참조

인라인 각주^[인라인 각주]

    인라인 각주 ^[각주 참조]

## Reference

➰ [마크다운 문법 1: 기본](https://velog.io/@gomsemarie/markdown)

➰ [마크다운 문법 2: 수식](https://khw11044.github.io/blog/blog-etc/2020-12-21-markdown-tutorial2/#2%EB%B6%84%EC%88%98--fraction)

➰ [마크다운 문법 3: 응용](https://gparkkii.github.io/tech/practical-markdown/)

# 🎇Obsidian only
## <span style="font-weight:600; font-style: italic;">Highlight</span>
==단어==

    ==단어==

## 2. 체크박스 <span style="font-weight:600; font-style: italic;">checkbox</span>
### 기본 체크 박스 (단축키 `Ctrl + L`)
- [ ] 기본 체크 박스 `- [ ]`

### 완료 체크 박스
- [-] 완료 체크 박스 `- [-]`

### 취소 체크 박스
- [x] 취소 체크 박스 `- [x]`

## 3. 그림 크기 <span style="font-weight:600; font-style: italic;">image size</span>

### `![대체 텍스트|가로x세로](이미지 URL)`

![위키피디아 흑요석|100x100](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/ObsidianOregon.jpg/360px-ObsidianOregon.jpg)
- `![위키피디아 흑요석|100x100](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/ObsidianOregon.jpg/360px-ObsidianOregon.jpg)`

## 4. 문서 링크 <span style="font-weight:600; font-style: italic;">internal link</span>

### 노트와 노트 또는 블록과 블록을 서로 링크
    [[노트 또는 블록 이름]]

[[README]] : `[[README]]`

## 5. 문서 임베딩 <span style="font-weight:600; font-style: italic;">embedding link</span>
###  `![[노트 이름]]` 노트 내용 자체를 다른 노트에 임베드
 
 - ![[Template]] : `![[Template]]`



## 6. 문단 참조 <span style="font-weight:600; font-style: italic;">block reference</span>
### `![[블록 링크 #^id]]` 다른 블록 내용을 현재 블록에 참조

- ![[Template#^sampleblock]] : `![[Template#^sampleblock]]`



## 7. 강조 상자 <span style="font-weight:600; font-style: italic;">callout</span>

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
