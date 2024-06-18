---
date:
  2024-06-13
tags:
  - DL
  - LLM
---

<link rel="stylesheet" href="../style.css" />

# LangChain

`TODO`: [langchain wikidocs](https://wikidocs.net/233341)

## LLM

- LLM 기반 애플리케이션을 구축하기 위해 고려해야할 요소
  - 매개 변수 조정
  - 프롬프트 보강
  - 응답 조정
- <span class="_yellow _600">특이점</span>
  - LLM은 상태를 저장하지 않으므로 대화의 이전 메시지를 기억할 수 없음
    - 개발자가 대화 기록을 유지하고 LLM에 컨텍스트를 제공해야함
  - LLM은 일률적인 규칙이 없음
    - 감정 분석, 분류, 질문 답변과 요약 등 다양한 시나리오에 특화된 여러 모델을 사용해야 함

## LLM 앱 구축을 위한 통합 API 레이어

- LangChain
  - LLM 애플리케이션을 위한 오픈소스 프레임워크
  - Python 및 Typescript 패키지 지원
  - 대규모 언어 모델 LLM 과 애플리케이션의 통합을 간소화하는 SDK
  - ODBC<sup>[1](#odbc)</sup>
    - 표준 SQL 문에 집중하게 함으로써 백엔드 데이터베이스의 구현 세부 정보를 요약하는 JDBC(Java Database Connectivity) 드라이버와 유사
  - 간단하고 통합된 API를 노출하여 기본 LLM의 구현 세부 사항을 요약함
  - 개발자들은 API를 통해 코드를 크게 변경하지 않고 모델을 쉽게 교체하거나 대체함

<figure>
    <img src="https://python.langchain.com/v0.2/svg/langchain_stack_dark.svg" alt="LangChain">
    <figcaption>[출처] Introduction | 🦜🧷LangChain python.langchain.com</figcaption>
</figure>

### LangChain의 LLM 흐름 조율

<img src="https://files.ciokorea.com/2023/08/Fixed_111.png" alt="LangChain의 LLM 조율 flow-chart">

- Data Sources 데이터 소스
  - 애플리케이션이 LLM 컨텍스트를 구축하기 위해 외부 소스(PDF, 웹페이지, CSV, 관계형 DB)에서 데이터를 검색해야 하는 경우가 있음
  - 서로 다른 소스에서 데이터에 접근하고 검색할 수 있는 모듈과 원활하게 통합됨
- Word Embeddings 단어 임베딩
  - 일부 외부 소스에서 검색된 데이터를 벡터로 변환한 후, 텍스트를 LLM 관련 단어 임베딩 모델에 전달 ($ex$ OpenAI CPT-3.5)
  - 선택한 LLM을 기반으로 최적의 임베딩 모델을 선택함
- Vector Database 벡터 데이터베이스
  - 생성된 임베딩은 유사성 검색을 위해 벡터 데이터베이스에 저장됨
  - 메모리 내 배열부터 파인콘 <span class="_pink _italic">Pinecone</span> 같은 호스팅 벡터 데이터베이스에 이르기까지 다양한 소스에서 벡터를 쉽게 저장하고 검색할 수 있도록 지원함
- LLM 대규모 언어 모델
  - OpenAI, 코히어 Cohere, AI21이 제공하는 주류 LLM과 허깅페이스 <span class="_pink _italic">Hugging Face</span>에서 제공하는 오픈소스 LLM을 지원

## LangChain 구조

<img src="https://files.ciokorea.com/2023/08/Fixed_222.png" alt="LangChain Framework Architecture">

### Model I/O

- 모델 입출력 모듈
- LLM과의 상호 작용
    - 효과적인 프롬프트 생성
    - 모델 API 호출
    - 결과 해석 지원

### Data Connection

- 데이터 연결 모듈
- LLM 애플리케이션의 ETL 파이프라인
    - 외부 문서(PDF, 엑셀 파일 etc.) 로드
    - 단어 임베딩 일괄 변환 및 벡터 데이터베이스 저장
    - 쿼리 검색
- LangChain의 가장 중요한 구성 요소

### Chains

- LLM과의 상호 작용
    - 유닉스 파이프라인 사용과 유사
    - 한 모듈의 출력이 다른 모듈에 입력으로 전송
    - 원하는 결과값을 얻기 위해 LLM의 여러 응답을 명확하게 요약해야 함
- 구성요소와 LLM을 활용하여 예상되는 응답을 얻는 효율적인 파이프라인을 구축하도록 설계됨
- 단순 구조 체인
    - 프롬프트와 LLM 포함
- 복잡 구조 체인
    - LLM을 여러 번 호출함(like 재귀 방식)
    - ①문서를 요약하고 내용에 대해 ②감정 분석을 수행하는 프롬프트

### Memory

- 메모리 모듈
- 상태 비저장형인 LLM 모델에 단기 및 장기 메모리를 추가할 수 있도록 함
- 단기 메모리 (간단한 메커니즘)
    - 대화 기록 유지
- 장기 메모리 (Redis 등 외부 소스)
    - 메시지 기록 저장

### Callbacks

- LLM 애플리케이션의 각 단계에 개발자가 연결할 수 있는 콜백 시스템
- 로깅, 모니터링, 스트리밍 및 기타 작업에 유용
- 사용자 지정 콜백 핸들러 작성
    - 파이프라인 내에서 특정 상황이 발생할 때 호출
- 기본 콜백
    - stdout
    - 모든 단계의 출력을 콘솔에 간단히 인쇄
    
### Agents

- 일종의 동적 체인
- LLM을 사용하여 프롬프트를 행동 계획으로 추출하는 ReAct 프롬프트(추론과 행동) 제작을 단순화
- 에이전트의 기본적인 동작을 선택하기 위해 LLM을 사용
    - 동작의 순서는 Chain(코드)을 통해 하드 코딩함
- 에이전트는 언어 모델을 추론엔진으로 사용
    - 어떤 순서로 어떤 동작을 취할지 결정함

---

## Tutorials

### Build a Simple LLM Application with LCEL

<details>
<summary class="_bgblue"></summary>

#### 1. Setup

```bash
pip install langchain
```

<span class="_pinksmall">💗 LangSmith Config 환경 변수 설정</span>

- CLI

```bash
export LANGCHAIN_TRACING_V2="true"
export LANGCHAIN_API_KEY="..."
```

- Source Code

```python
import getpass
import os

os.environ["LANGCHAIN_TRACING_V2"] = "true"
os.environ["LANGCHAIN_API_KEY"] = getpass.getpass()
# os.environ["LANGCHAIN_API_KEY"] = "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"
```

#### 2. Using Language Models

<details>
  <summary class="_pinksmall">Cohere?</summary>

- Langchain Docs Tutorial의 Default 모델은 OpenAI `gpt-4.0`
  - OpanAI API 기본 credit 3개월 기한 만료로 사용 불가
- 대신 <span class="_yellow _italic">Cohere Trial Key</span> 발급 사용
</details>

```bash
pip install -qU langchain-cohere
```

```python
import getpass
import os

os.environ["COHERE_API_KEY"] = getpass.getpass()
# os.environ["COHERE_API_KEY"] = "4BtT07HreyYG7DWIcOPYRq5jwU5lBEW8VkUzaBNZ"
```

```python
from langchain_cohere import ChatCohere

# LangChain "Runnables"
model = ChatCohere(model="command-r")
```

```python
from langchain_core.messages import HumanMessage, SystemMessage

messages = [
    SystemMessage(content="Translate the following from English into Italian"),
    HumanMessage(content="hi!"),
]

model.invoke(messages)
```

```python
AIMessage(content='ciao!', response_metadata={'token_usage': {'completion_tokens': 3, 'prompt_tokens': 14, 'total_tokens': 17}, 'model_name': 'command-r'}, id='run-c8071053-c633-4d16-b356-68e632731c71')
```

#### 3. OutputParsers
```python
from langchain_core.output_parsers import StrOutputParser

result = model.invoke(message)

parser = StrOutputParser()
parser.invoke(result)
```
```python
'Ciao!'
```
```python
chain = model | parser
chain.invoke(message)
```
```python
'Ciao!'
```

#### 4. Prompt Templates
- `language`: The language to translate text into
- `text`: The text to translate

```python
from langchain_core.prompts import ChatPromptTemplate

system_template = "Translate the following into {language}:"
prompt_template = ChatPromptTemplate.from_messages(
  [("system", system_template), ("user", "{text}")]
)

result = prompt_template.invoke({"language": "italian", "text": "hi"})
```
```python
ChatPromptValue(messages=[SystemMessage(content='Translate the following into italian:'), HumanMessage(content='hi')])
```
```python
result.to_messages()
```
```python
[SystemMessage(content='Translate the following into italian:'),
 HumanMessage(content='hi')]
```

#### 5. Chaining together components with LCEL
- `LangChain Expression Language`
- The `|` operator is used in LangChain to combine two elements together.

```python
chain = prompt_template | model | parser
chain.invoke({"language": "italian", "text": "hi"})
```
```python
'Ciao!'
```

#### Final

```bash
pip install langchain
pip install -qU langchain-cohere
```

```python
import os

# 환경변수 구성
os.environ["LANGCHAIN_TRACING_V2"] = "true"
os.environ["LANGCHAIN_API_KEY"] = "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"
os.environ["COHERE_API_KEY"] = "4BtT07HreyYG7DWIcOPYRq5jwU5lBEW8VkUzaBNZ"

from langchain_cohere import ChatCohere
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser

# 프롬프트 템플릿 작성
system_template = "Translate the following into {language}:"
prompt_template = ChatPromptTemplate.from_messages(
    [("system", system_template), ("user", "{text}")]
)
# 모델 객체 호출
model = ChatCohere(model="command-r")
# 응답 파싱 객체 생성
parser = StrOutputParser()
# 체인 구성
chain = prompt_template | model | parser
# 체인 실행
chain.invoke({"language": "italian", "text": "hi"})
```

```python
'Ciao!'
```
</details>


### Build a Chatbot

#### Concepts

- Have a conversation
- Remember previous interactions

<details>
  <summary class="_bgblue"></summary>

#### 1. Setup

</details>

### Build a Retrieval Augmented Generation (RAG) App

<details>
  <summary class="">💫 RAG이란?</summary>
  LLM 지식을 추가 데이터로 보강하는 기술 <span class="_pinksmall">검색 증강 생성</span><br>
  LLM은 광범위한 주제에 대해 추론할 수 있지만, 지식은 학습된 특정 시점까지의 공개 데이터에 국한됨<br>
  개인 데이터 또는 모델의 컷오프 날짜 이후에 도입된 데이터에 대해 추론할 수 있는 AI 애플리케이션을 구축하려면 모델에 필요한 특정 정보로 모델의 지식을 보강해야 함<br>
  적절한 정보를 가져와 모델 프롬프트에 삽입하는 과정<br>
  → <code>RAG</code>(Retrieve Augmented Generation)
</details>

#### Concepts

- **Indexing**
  - 1. *Load*
  - 2. *Split*
  - 3. *Store*
- **Retrieval and generation**
  - 4. *Retrieval*
  - 5. *Generation*

#### Setup

<details>
  <summary class="_bgblue"></summary>

```bash
pip install langchain
pip install -qU langchain-cohere
```
```python
import os

os.environ["LANGCHAIN_TRACING_V2"] = "true"
os.environ["LANGCHAIN_API_KEY"] = "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"
os.environ["COHERE_API_KEY"] = "4BtT07HreyYG7DWIcOPYRq5jwU5lBEW8VkUzaBNZ"
```
</details>

#### Preview

<details>
  <summary class="_bgblue"></summary>

```python
from langchain_cohere import ChatCohere

llm = ChatCohere(model="command-r")
```

```python
import bs4
from langchain import hub
from langchain_chroma import Chroma
from langchain_community.document_loaders import WebBaseLoader
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnablePassthrough
from langchain_cohere import CohereEmbeddings
from langchain_text_splitters import RecursiveCharacterTextSplitter

# Load, chunk and index the contents of the blog.
loader = WebBaseLoader(
    web_paths=("https://lilianweng.github.io/posts/2023-06-23-agent/",),
    bs_kwargs=dict(
        parse_only=bs4.SoupStrainer(
            class_=("post-content", "post-title", "post-header")
        )
    ),
)
docs = loader.load()

text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
splits = text_splitter.split_documents(docs)
vectorstore = Chroma.from_documents(documents=splits, embedding=CohereEmbeddings())

# Retrieve and generate using the relevant snippets of the blog.
retriever = vectorstore.as_retriever()
prompt = hub.pull("rlm/rag-prompt")


def format_docs(docs):
    return "\n\n".join(doc.page_content for doc in docs)


rag_chain = (
    {"context": retriever | format_docs, "question": RunnablePassthrough()}
    | prompt
    | llm
    | StrOutputParser()
)

rag_chain.invoke("What is Task Decomposition?")
```

```python
# cleanup
vectorstore.delete_collection()
```
</details>

#### Detailed Walkthrough
<details>
  <summary class="_bgblue"></summary>


##### 1. Indexing: Load

```python
import bs4
from langchain_community.document_loaders import WebBaseLoader

# Only keep post title, headers, and content from the full HTML.
bs4_strainer = bs4.SoupStrainer(class_=("post-title", "post-header", "post-content"))
loader = WebBaseLoader(
    web_paths=("https://lilianweng.github.io/posts/2023-06-23-agent/",),
    bs_kwargs={"parse_only": bs4_strainer},
)
docs = loader.load()

len(docs[0].page_content)
```

```python
print(docs[0].page_content[:500])
```

##### 2. Indexing: Split

```python
from langchain_text_splitters import RecursiveCharacterTextSplitter

text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000, chunk_overlap=200, add_start_index=True
)
all_splits = text_splitter.split_documents(docs)

len(all_splits)
```
```python
len(all_splits[0].page_content)
```
```python
all_splits[10].metadata
```
##### 3. Indexing: Store
```python
from langchain_chroma import Chroma
from langchain_openai import OpenAIEmbeddings

vectorstore = Chroma.from_documents(documents=all_splits, embedding=OpenAIEmbeddings())
```

##### 4. Retrieval and Generation: Retrieve
```python
retriever = vectorstore.as_retriever(search_type="similarity", search_kwargs={"k": 6})

retrieved_docs = retriever.invoke("What are the approaches to Task Decomposition?")

len(retrieved_docs)
```
```python
print(retrieved_docs[0].page_content)
```

##### 5. Retrieval and Generation: Generate
```bash
pip install langchain
pip install -qU langchain-cohere
```
```python
import os

os.environ["LANGCHAIN_TRACING_V2"] = "true"
os.environ["LANGCHAIN_API_KEY"] = "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"
os.environ["COHERE_API_KEY"] = "4BtT07HreyYG7DWIcOPYRq5jwU5lBEW8VkUzaBNZ"
```
```python
from langchain_cohere import ChatCohere

llm = ChatCohere(model="command-r")
```
```python
from langchain import hub

prompt = hub.pull("rlm/rag-prompt")

example_messages = prompt.invoke(
    {"context": "filler context", "question": "filler question"}
).to_messages()

print(example_messages)
```
```python
print(example_messages[0].content)
```
</details>


# *Reference*

[LangChain 칼럼](https://www.ciokorea.com/column/305341#csidxb23f99cd3f147a1b27c8968f282a5a0 )  
[LangChain Docs](https://python.langchain.com/v0.2/docs/introduction/)

# *Footnotes*

<a name="odbc" class="_footnote">1.</a> Open DataBase Connectivity  
응용프로그램 <span class="_pink _italic">HeidiSQL, SQLyog, etc.</span>에서 데이터 접근 할 때 어떠한 DBMS <span class="_pink _italic">Oracle, MySQL, MariaDB, etc.</span>에 의해 관리되고 있는지 의식할 필요가 없음
