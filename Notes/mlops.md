---
tags:
  - AI
  - ML
  - MLOPS
---
# 🌟 MLOps
➰　기계 학습 개발 운영 방식

## ✔ MLOps 정의
### 📌 DevOps
Devalopment Operations <br>
개발과 운영을 나누지 않고 개발의 생산성과 운영의 안정성을 최적화하기 위한 방법론

### 📌 MLOps
DevOps를 ML 시스템에 적용 → The AI Lifecycle for IT Production <br>
**MLOps는 ML의 전체 Life cycle을 관리해야 한다.**

MLOps란 단순히 ML 모델 뿐만 아니라 <br>
데이터를 수집하고 분석하는 단계, ML 모델을 학습하고 배포하는 단계까지 전 과정을 AI Lifecycle로 보고 <br>
MLOps의 대상으로 삼는다.

## ⚡ML 시스템의 요소
머신러닝 시스템을 프로덕션 환경에 적용하고 운영하기 위해서는, <br>
머신러닝 모델 뿐만 아니라 기반 데이터와 인프라를 포함한 모든 시스템이 유기적으로 돌아가야 한다. <br>
ML 시스템의 운영을 위해 DevOps의 원칙을 적용한 것이 MLOps다.

## DevOps vs MLOps
MLOps과 소프트웨어 시스템의 차이점
- Testing
  일반적인 단위, 통합 테스트 외에 데이터 검증, 학습된 모델 품질 평가, 모델 검증이 추가로 필요하다.
- Deployment
  오프라인에서 학습된 ML모델을 배포하는 수준에 그치는 것이 아니라, 새 모델을 재학습하고, 검증하는 과정을 자동화해야 한다.
- Production
  일반적으로 알고리즘과 로직의 최적화를 통해 최적의 성능을 낼 수 있는 소프트웨어 시스템과 달리, ML 모델은 이에 대해 지속적으로 진화하는 data profile 자체만으로 성능이 저하될 수 있다.
  즉 기존 소프트웨어 시스템보다 더 다양한 이유로 성능이 손상될 수 있으므로,
  데이터의 summary statistics를 꾸준히 추적하고 모델의 온라인 성능을 모니터링하여 값이 기대치를 벗어나면 알림을 전송하거나 롤백할 수 있어야 한다.
- CI (Continuous Integration)
  CI는 code와 components 뿐만 아니라 data, data schema, model에 대해 모두 테스트되고 검증되어야 한다.
- CD (Continuous Delivery)
  단일 소프트웨어 패키지가 아니라 ML 학습 파이프라인 전체를 배포해야 한다.
- CT (Continuous Training)
  ML 시스템 만의 속성으로, 모델을 자동으로 학습시키고 평가하는 단계이다.

## Data science steps for ML
> Business use case와 Success criteria를 설정한 후 ML 모델을 프로덕션에 배포하기 까지,
> 모든 ML 프로젝트이 수반하는 과정
1. DataExtraction 데이터 추출
   데이터 소스에서 관련 데이터 추출
2. Data Analysis 데이터 분석
   데이터의 이해를 위한 탐사적 데이터 분석(EDA) 수행
   모델에 필요한 데이터 스키마 및 특성 이해
3. Data Preparation 데이터 준비
   데이터의 학습, 검증, 테스트 세트 분할
4. Model Training 모델 학습
   다양한 알고리즘 구현, 하이퍼 파라미터 조정 및 적용
   output → 학습된 모델
5. Model Evaluation 모델 평가
   holdout test set에서 모델을 평가
   output → 모델 성과 평가 metric
6. Model Validation 모델 검증
   기준치 이상의 모델 성능이 검증되는지, 배포에 적합한 수준인지 검증
7. Model Serving 모델 서비스
   온라인 예측을 제공하기 위한 REST API가 포함된 마이크로 서비스
   배치 예측 서비스
   모바일 서비스의 embedded 모델
8. Model Monitoring 모델 모니터링
   모델의 예측 성능을 모니터링

## 📌MLOps level 0: Manual Process
데이터 추출과 분석, 모델 학습, 검증을 포함한 모든 단계가 수동으로 이루어진다.
- ML과 Operation간 disconnection
	- 데이터 사이언티스트가 모델을 아티팩트로 전달하고 엔지니어가 low latency로 프로덕션 환경에 배포한다. training-serving skew가 발생할 수 있다.
- Infrequent release iteration
	- 새 모델 버전의 배포가 드문드문 비정기적으로 발생한다.
- No CI
	- 변경이 많지 않아 CI가 고려되지 않는다. 스크립트 수행이나 노트북에서 개인적으로 테스트를 수행한다.
- No CD
	- 배포가 드물기 때문에 CD가 불필요하다.
- Active performance monitoring의 부재
	- 로그나 모델의 예측 성능 등을 모니터링하지 않는다. 모델의 성능이 저하되거나 모데링 이상동작 하는 것을 감지할 수 없다.

> 🔥 Level 0의 Challenges <br>
> 모델이 프로덕션 환경에 배포될 때 실제 환경과 데이터 변화 등으로 인해 실패할 수 있다.
> 모델의 정확도를 프로덕션 환경에서도 유지하기 위해 다음의 노력을 기울여야 한다.
> - 프로덕션 환경의 모델 품질 모니터링
>   성능 저하 및 모델의 staleness(신선도 저하)를 감지
> - 최신 데이터로 모델을 재학습
> - 새로운 feature의 추출, 모델 아키텍처, 하이퍼파라미터 등의 새로운 구현을 계속 시도

## 📌MLOps level 1: ML Pipeline Automation
Level 1의 목표는 ML 파이프라인을 자동화하여 모델을 지속적을 학습시키는 것이다.

### Level 1의 특징
- Rapid Experiment
  실험을 빠르게 반복하고 전체 파이프라인을 프로덕션에 빠르게 배포한다.
- 개발 환경에서 쓰인 파이프라인이 운영 환경에 그대로 사용된다. → ***DevOps의 MLOps 통합의 핵심 요소***
- 프로덕션 모델 CT (Continuous Training)
  새로운 데이터를 사용하여 프로덕션 모델이 자동으로 학습한다.
- CD (Continuous Delivery)
  새로운 데이터로 학습하고 검증한 모델을 지속적으로 배포한다.
- Level 0는 학습된 모델만 배포한 반면, Level1은 전체 파이프라인을 배포한다.

> [!tip] CT를 위한 요구 조건
> 새로운 데이터를 통해 새로운 모델을 지속적으로 학습하므로, Data Validation과 Model Validation이 필수적이다.

### Data and Model Validation
- Data validation
  데이터 검증이 실패하면 신규 모델의 배포를 중지한다. 배포 중지의 의사결정까지 자동화되어야 한다.
  - Data schema skews
    예상치 못한 데이터가 생성된 경우, 예상 범주를 벗어난 특성이 생성된 경우 등
  - Data values skews
    데이터의 통계적 속성이 변화하고 있음을 감지해야 한다. 변화를 감지하여 모델의 재학습을 트리거한다.
- Modle Validation
  모델이 새로운 데이터로 재학습을 마치고, 운영 환경에 반영되기 전 평가 및 검증되어야 한다.
  - Test dataset으로 평가 metric을 생성한다.
  - 현재 모델과 새 모델의 평가 metric을 비교 검증한다.
  - 새로운 모델의 성능이 다양한 segment에서 일관된 성과를 보이는지 검증한다.
  - 인프라 및 예측 서비스 API와 호환성 테스트를 수행한다.

### Feaure Store
> Feature store는 학습과 서비스 제공에 사용하는 모든 features를 모아둔 저장소이다.
> 대용량 배치 처리와 low latency의 실시간 서비스 제공을 모두 지원할 수 있어야 한다.
- 사용가능한 모든 feature의 저장소
- 항상 최신화된 데이터

### Metadata Management
> ML 파이프라인의 실행 정보, 데이터 및 아티팩트의 계보등을 저장한다.
- 실행된 파이프라인 버전, 시작-종료 시간, 소요 시간 등
- 파이프라인의 실행자, 매개변수 인수
- 이전 모델에 대한 포인터(모델 롤백이 필요한 경우)
- 모델 평가 단계에서 생성된 모델 평가 측정 항목
### ML Pipeline Trigger
- 모델 재학습 파이프라인의 자동화
- 재학습 빈도는 데이터 패턴의 변경 빈도와 모델 재학습 비용에 따라 달라진다.
- 모델 성능 저하가 감지된 경우 모델 재학습을 트리거한다.

## 📌MLOps level 2: CI/CD Pipeline Automation
Level1보다 CI/CD 면에서 집중적으로 강화된 시스템
→ ***CI/CD 자동화 파이프라인***

### Continuous Integration
> 파이프라인과 구성요소는 커밋 혹은 푸시될 때 빌드, 테스트, 패키징된다.
- 특성 추출 로직 테스트
- 모델 구현 메소드 단위 테스트
- 모델 학습 수렴 여부 테스트
- 모델 학습 NaN 생성 여부 테스트
- 파이프라인 구성요소별 아티팩트 생성 테스트
- 파이프라인 구성요소간 통합 테스트

### Continuous Delivery
- 모델 배포 전 모델과 대상 인프라 호환성 확인
  패키지 호환 여부/메모리/컴퓨팅 자원 등
- 서비스 API 호출 테스트
- QPS 및 지연 시간 등 서비스 부하 테스트

## 📍 ML 기반 시스템의 테스트와 모니터링
MLOps는 소프트웨어 레벨의 유닛, 통합 테스트 뿐만 아니라 데이터와 모델의 성능까지 테스트하고 검증해야 하기 때문에 DevOps보다 복잡하고 많은 테스트를 필요로 한다.

---
### [ClearML]

1. `$ pip install clearml`
2. `$ clearml-init`
3. And, input credential at console.
4. Add clearml snippet in your code.
	```python
	from clearml import Task
	
	task = Task.init(project_name="my_project", task_name="my_task")
	```

## Use Local Host Server

> [🔥] Linux <br>
> ***Stop*** <br>
> `$ docker-compose -f /opt/clearml/docker-compose.yml down` <br>
> ***Restart*** <br>
> `$ docker-compose -f /opt/clearml/docker-compose.yml up -d`
