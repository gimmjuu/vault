---
tags:
  - DL
  - LLM
---
<style type='text/css'>
    /* [class*="box"] { display: flex; width: 20%; height: 50px; } */
    img { width: 40%; margin: 10 0; border-radius:3px; }
    [class*="yw"] { color:rgba(255,255,155,0.8); }
</style>

# LangChain
## LLM
- LLM 기반 애플리케이션을 구축하기 위해 고려해야할 요소
    - 매개 변수 조정
    - 프롬프트 보강
    - 응답 조정
- <span style="color:rgba(255,255,155,0.8);font-weight:500;">Especially</span>
    - LLM은 상태를 저장하지 않으므로 대화의 이전 메시지를 기억할 수 없음
        - 개발자가 대화 기록을 유지하고 LLM에 컨텍스트를 제공해야함
    - LLM은 일률적인 규칙이 없음
        - 감정 분석, 분류, 질문 답변과 요약 등 다양한 시나리오에 특화된 여러 모델을 사용해야 함

## LLM 앱 구축을 위한 통합 API 레이어
- LangChain
    - LLM 애플리케이션을 위한 오픈소스 프레임워크
    - Python 및 Typescript 패키지 지원
    - 대규모 언어 모델 LLM 과 애플리케이션의 통합을 간소화하는 SDK
    - ODBC<sup>[1](#odbc)</sup> (Open DataBase Connectivity)
        - 또는 표준 SQL 문에 집중하게 함으로써 백엔드 데이터베이스의 구현 세부 정보를 요약하는 JDBC(Java Database Connectivity) 드라이버와 유사
    - 간단하고 통합된 API를 노출하여 기본 LLM의 구현 세부 사항을 요약함
    - 개발자들은 API를 통해 코드를 크게 변경하지 않고 모델을 쉽게 교체하거나 대체함

<figure>
<img src="https://python.langchain.com/v0.2/svg/langchain_stack_dark.svg" alt="출처|https://python.langchain.com/v0.2/docs/introduction/" alt="Image">
<figcaption>[출처] Introduction | 🦜🧷LangChain python.langchain.com</figcaption>
</figure>

### LangChain의 LLM 흐름 조율
<center>
<img src="https://files.ciokorea.com/2023/08/Fixed_111.png" alt="LangChain의 LLM 조율 flow-chart">
</center>

- Data Sources 데이터 소스
    - 애플리케이션이 LLM 컨텍스트를 구축하기 위해 외부 소스(PDF, 웹페이지, CSV, 관계형 DB)에서 데이터를 검색해야 하는 경우가 있음
    - 서로 다른 소스에서 데이터에 접근하고 검색할 수 있는 모듈과 원활하게 통합됨
- Word Embeddings 단어 임베딩
    - 일부 외부 소스에서 검색된 데이터를 벡터로 변환한 후, 텍스트를 LLM 관련 단어 임베딩 모델에 전달 ($ex$ OpenAI CPT-3.5)
    - 선택한 LLM을 기반으로 최적의 임베딩 모델을 선택함
- Vector Database 벡터 데이터베이스
    - 생성된 임베딩은 유사성 검색을 위해 벡터 데이터베이스에 저장됨
    - 메모리 내 배열부터 파인콘 <span style="color:rgba(255,255,155,0.8);font-style:italic;">Pinecone</span> 같은 호스팅 벡터 데이터베이스에 이르기까지 다양한 소스에서 벡터를 쉽게 저장하고 검색할 수 있도록 지원함
- LLM 대규모 언어 모델
    - OpenAI, 코히어 Cohere, AI21이 제공하는 주류 LLM과 허깅페이스 <span style="color:pink;font-style:italic;">Hugging Face</span>에서 제공하는 오픈소스 LLM을 지원

### LangChain 구조
<center>
<img src="https://files.ciokorea.com/2023/08/Fixed_222.png" alt="LangChain Framework Architecture">
</center>

- Model I/O
    - 모델 입출력 모듈
    - LLM과의 상호 작용
        - 효과적인 프롬프트 생성
        - 모델 API 호출
        - 결과 해석 지원
- Data Connection
    - 데이터 연결 모듈
    - LLM 애플리케이션의 ETL 파이프라인
        - 외부 문서(PDF, 엑셀 파일 etc.) 로드
        - 단어 임베딩 일괄 변환 및 벡터 데이터베이스 저장
        - 쿼리 검색
    - LangChain의 가장 중요한 구성 요소
- Chains
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
- Memory
    - 메모리 모듈
    - 상태 비저장형인 LLM 모델에 단기 및 장기 메모리를 추가할 수 있도록 함
    - 단기 메모리 (간단한 메커니즘)
        - 대화 기록 유지
    - 장기 메모리 (Redis 등 외부 소스)
        - 메시지 기록 저장
- Callbacks
    - LLM 애플리케이션의 각 단계에 개발자가 연결할 수 있는 콜백 시스템
    - 로깅, 모니터링, 스트리밍 및 기타 작업에 유용
    - 사용자 지정 콜백 핸들러 작성
        - 파이프라인 내에서 특정 상황이 발생할 때 호출
    - 기본 콜백
        - stdout
        - 모든 단계의 출력을 콘솔에 간단히 인쇄
- Agents
    - 일종의 동적 체인
    - LLM을 사용하여 프롬프트를 행동 계획으로 추출하는 ReAct 프롬프트(추론과 행동) 제작을 단순화
    - 에이전트의 기본적인 동작을 선택하기 위해 LLM을 사용
        - 동작의 순서는 Chain(코드)을 통해 하드 코딩함
    - 에이전트는 언어 모델을 추론엔진으로 사용
        - 어떤 순서로 어떤 동작을 취할지 결정함

# *Footnotes*
<a name="odbc">1.</a> *Open DataBase Connectivity*, 응용프로그램 <span style="color:pink;font-style:italic;">HeidiSQL, SQLyog, etc.</span>에서 데이터 접근 할 때 어떠한 DBMS <span style="color:pink;font-style:italic;">Oracle, MySQL, MariaDB, etc.</span>에 의해 관리되고 있는지 의식할 필요가 없음

# *Reference*
[참조](https://www.ciokorea.com/column/305341#csidxb23f99cd3f147a1b27c8968f282a5a0 )

