# PyMC Examples

Examples of Bayesian inference and Gaussian Process modelling with PyMC. Use Python 3.11 or later and uv, following [pyproject.toml](pyproject.toml):

```bash
git clone https://github.com/bet-lab/pymc-examples.git
cd pymc-examples
uv sync
uv run jupyter notebook
```

Start with [the beginner guide](BEGINNER_GUIDE.md), then open [the toy-model notebook](examples/pymc-gp-toy_model.ipynb). The detailed Korean setup and learning guide is preserved below.

## Korean reference guide

PyMC를 활용한 Bayesian 추론과 Gaussian Process 모델링 예제 저장소다.

## 👋 처음 오셨나요?

**초보자분들을 위한 상세한 가이드**를 준비했습니다! Bayesian 추론이나 Gaussian Process가 처음이시라면, [초보자 가이드 (BEGINNER_GUIDE.md)](BEGINNER_GUIDE.md)를 먼저 읽어보시기 바랍니다. 핵심 개념부터 단계별 학습 경로까지 친절하게 설명되어 있습니다.

## 📋 목차

- [환경 설치](#-환경-설치)
- [프로젝트 구조](#-프로젝트-구조)
- [예제 소개](#-예제-소개)
- [기술적 배경](#-기술적-배경)
- [사용법](#-사용법)
- [주요 개념 설명](#-주요-개념-설명)
- [의존성](#-의존성)

## 🚀 환경 설치

### 1. 저장소 클론
```bash
git clone https://github.com/bet-lab/pymc-examples.git
cd pymc-examples
```

### 2. uv 설치

이 프로젝트는 `uv`를 사용하여 패키지 의존성을 관리한다. `uv`는 Python 패키지 설치와 가상환경 관리를 빠르고 효율적으로 처리하는 도구다.

#### Linux/Mac에서 uv 설치
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

#### Windows에서 uv 설치
Windows에서는 PowerShell을 관리자 권한으로 실행한 후 다음 명령어를 입력한다:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

또는 Windows 패키지 관리자 `winget`을 사용할 수도 있다:
```powershell
winget install --id=astral-sh.uv -e
```

설치 후 터미널을 재시작하여 `uv` 명령어를 사용할 수 있는지 확인한다:
```bash
uv --version
```

### 3. 프로젝트 의존성 설치

프로젝트 디렉토리에서 다음 명령어를 실행하여 필요한 모든 패키지를 설치하고 가상환경을 생성한다:

```bash
uv sync
```

`uv sync` 명령어는 다음 작업을 자동으로 수행한다:
- `pyproject.toml` 파일을 읽어 필요한 패키지 목록 확인
- `.venv` 디렉토리에 가상환경 생성 (없는 경우)
- 모든 의존성 패키지를 가상환경에 설치
- 패키지 버전을 `uv.lock` 파일에 고정

### 4. 가상환경 활성화

설치가 완료되면 가상환경을 활성화한다:

#### Linux/Mac
```bash
source .venv/bin/activate
```

#### Windows (PowerShell)
```powershell
.venv\Scripts\Activate.ps1
```

#### Windows (CMD)
```cmd
.venv\Scripts\activate.bat
```

가상환경이 활성화되면 터미널 프롬프트 앞에 `(.venv)`가 표시된다.

### 5. Jupyter Notebook 실행

가상환경이 활성화된 상태에서 Jupyter Notebook을 실행한다:

```bash
jupyter notebook
```

또는 가상환경 활성화 없이 바로 실행할 수도 있다:
```bash
uv run jupyter notebook
```

브라우저가 자동으로 열리면 `examples/pymc-gp-toy_model.ipynb` 파일을 선택하여 예제를 실행할 수 있다.

## 📁 프로젝트 구조

```
pymc-examples/
├── README.md                    # 프로젝트 설명서
├── pyproject.toml              # 프로젝트 설정 및 의존성
├── uv.lock                     # 의존성 버전 잠금 파일
├── examples/                   # 예제 노트북 폴더
│   └── pymc-gp-toy_model.ipynb # Gaussian Process 예제
└── dataset/                    # 데이터셋 폴더
    ├── datacomp_hourly.csv     # 컴퓨터 시뮬레이션 데이터
    └── datafield_hourly.csv    # 실제 관측 데이터
```

## 📊 예제 소개

### Gaussian Process Toy Model (`pymc-gp-toy_model.ipynb`)

이 예제는 PyMC를 사용한 Gaussian Process 기반 Bayesian 추론의 완전한 구현을 제공한다. 컴퓨터 시뮬레이션 데이터와 실제 관측 데이터를 결합하여 물리적 파라미터를 추정하는 방법을 단계별로 설명한다.

#### 🎯 주요 목표
- 컴퓨터 시뮬레이션 데이터와 실제 관측 데이터를 통합
- Gaussian Process를 에뮬레이터로 활용
- 물리적 파라미터(기기밀도, 조명밀도, COP)의 Bayesian 추정 및 불확실성 정량화

#### 📈 데이터 모델

실제 측정 데이터 $z$는 다음과 같이 모델링된다:

$$z = \eta(\mathbf{x}, \boldsymbol{\theta}) + e$$

여기서:
- $\eta(\mathbf{x}, \boldsymbol{\theta})$: 실제 물리 프로세스 또는 컴퓨터 시뮬레이션 함수
- $\mathbf{x}$: 입력 변수 (외기온도, 상대습도)
- $\boldsymbol{\theta}$: 모델 파라미터 (기기밀도, 조명밀도, COP)
- $e$: 관측 오차 (랜덤 노이즈)

#### 🔬 데이터 타입

**1. 컴퓨터 시뮬레이션 데이터 (`datacomp_hourly.csv`)**
- 형태: `(yc, xc1, xc2, tc1, tc2, tc3)`
- `yc`: 에너지 사용량 (시뮬레이션 출력값)
- `xc1`: 외기온도 (첫 번째 입력 변수)
- `xc2`: 상대습도 (두 번째 입력 변수)
- `tc1`: 기기밀도 (추정 대상 파라미터)
- `tc2`: 조명밀도 (추정 대상 파라미터)
- `tc3`: COP - Coefficient of Performance (추정 대상 파라미터)

**2. 실제 관측 데이터 (`datafield_hourly.csv`)**
- 형태: `(yf, xf1, xf2)`
- `yf`: 에너지 사용량 (실제 측정값)
- `xf1`: 외기온도 (첫 번째 입력 변수)
- `xf2`: 상대습도 (두 번째 입력 변수)
- 물리적 파라미터 `tc1, tc2, tc3`는 미지수로 Bayesian 추론을 통해 추정

## 🧮 기술적 배경

### Bayesian Model Calibration

이 예제는 Kennedy and O'Hagan (2001)의 Bayesian Model Calibration 방법론을 기반으로 한다:

1. **완벽한 모델 가정**: 컴퓨터 모델이 실제 물리 프로세스를 완벽하게 재현한다고 가정
2. **Gaussian Process 에뮬레이션**: 복잡한 시뮬레이션 함수를 GP로 근사
3. **통합된 추론**: 시뮬레이션과 관측 데이터를 하나의 모델로 통합

### Gaussian Process 모델링

**커널 함수**: Exponential Quadratic (RBF) 커널 사용

$$k(x_i, x_j) = \eta^2 \exp\left(-\frac{1}{2}\sum_{d=1}^{5}\frac{(x_{i,d} - x_{j,d})^2}{l_d^2}\right)$$

**하이퍼파라미터**:
- `l`: Length-scale 파라미터 (각 입력 차원에서의 함수 변동성)
- `eta`: Amplitude 파라미터 (함수 값의 전체적인 변동 폭)
- `sigma`: 관측 노이즈의 표준편차

### MCMC 샘플링

**NUTS (No-U-Turn Sampler)** 사용:
- Hamiltonian Monte Carlo의 개선된 버전
- 자동 튜닝과 효율적인 수렴
- NumPyro 백엔드로 빠른 성능 제공

## 💻 사용법

### 1. 노트북 실행
```bash
# Jupyter Notebook에서 예제 파일 열기
jupyter notebook examples/pymc-gp-toy_model.ipynb
```

### 2. 단계별 실행

노트북은 다음과 같은 단계로 구성되어 있다:

1. **라이브러리 임포트 및 환경 설정**
   - PyMC, NumPy, ArviZ 등 필수 라이브러리 임포트
   - 각 라이브러리의 역할과 사용 목적 설명

2. **데이터 임포트 및 탐색적 분석**
   - 컴퓨터 시뮬레이션 데이터와 실제 관측 데이터 로드
   - 데이터 구조 확인 및 기본 통계량 분석

3. **데이터 시각화 및 탐색적 분석**
   - 실제 관측 데이터의 2차원 공간 분포 시각화
   - 등고선 플롯을 통한 에너지 소비 패턴 파악

4. **Bayesian Inference 모델 정의**
   - 데이터 정규화 (Min-Max Scaling)
   - GP 커널 및 하이퍼파라미터 정의
   - Marginal GP를 활용한 효율적인 모델 구성

5. **MCMC 샘플링 수행**
   - NUTS 샘플러로 사후 분포 근사
   - 샘플링 파라미터 설정 및 수렴성 확인

6. **사후 분포 분석 및 시각화**
   - Trace plot으로 MCMC 수렴성 진단
   - Length-scale 해석을 통한 변수 중요도 분석
   - Pair plot으로 파라미터 간 상관관계 분석

7. **사후 예측 분포 (Posterior Predictive Distribution)**
   - 새로운 위치에서의 예측 수행
   - 95% 신뢰구간과 예측 평균 시각화

### 3. 결과 해석

#### 파라미터 추정 결과
- `theta_true[0]`: 기기밀도 (Equipment Density)
- `theta_true[1]`: 조명밀도 (Lighting Density)  
- `theta_true[2]`: COP (Coefficient of Performance)

#### Length-scale 해석
- **큰 length-scale**: 해당 차원에서 입력 변화에 대한 출력 변화가 작음 (낮은 민감도)
- **작은 length-scale**: 해당 차원이 출력에 큰 영향을 미침 (높은 민감도)

예제에서는 공간 변수(외기온도, 상대습도)의 length-scale이 파라미터보다 작은 경향을 보이는데, 이는 온도와 습도가 에너지 소비량에 더 큰 영향을 미친다는 것을 의미한다.

## 📚 주요 개념 설명

### 1. 데이터 정규화 (Data Normalization)

**왜 필요한가?**
- Gaussian Process 모델링에서 수치적 안정성 확보
- 서로 다른 스케일의 변수들을 공정하게 비교
- MCMC 샘플링의 수렴 속도 향상

**Min-Max Scaling 공식:**
$$x_{normalized} = \frac{x - x_{min}}{x_{max} - x_{min}}$$

모든 데이터를 [0, 1] 범위로 변환하여 동일한 스케일에서 처리한다.

### 2. Marginal Gaussian Process

**Latent GP vs Marginal GP**

- **Latent GP**: GP에서 함수값 $\mathbf{f}$를 먼저 샘플링한 후 관측값 예측
- **Marginal GP**: 중간 변수 $\mathbf{f}$를 적분으로 제거하여 직접 관측값 모델링

**장점:**
- 계산 효율성 향상 (차원 축소)
- 수치적 안정성 증가
- 정규분포 노이즈 가정 하에서 해석적 marginal likelihood 계산 가능

### 3. MCMC와 NUTS

**MCMC (Markov Chain Monte Carlo)**란?
- 복잡한 다차원 사후 분포에서 샘플을 생성하는 알고리즘
- 직접 계산이 어려운 확률 분포를 근사

**NUTS (No-U-Turn Sampler)**의 특징:
- Hamiltonian Monte Carlo의 개선 버전
- Step size와 integration time을 자동으로 조정
- 다른 MCMC 방법보다 빠르고 효율적인 수렴

**주요 파라미터:**
- `draws`: 실제 샘플링할 개수 (사후 분포 근사용)
- `tune`: 워밍업 단계 길이 (샘플러 파라미터 튜닝)
- `chains`: 독립적인 샘플링 체인 수 (수렴성 진단용)
- `target_accept`: 목표 수락률 (0.8-0.95 권장)

### 4. 사후 분포 진단

**Trace Plot**: 
- 왼쪽: 사후 분포 히스토그램
- 오른쪽: MCMC 샘플링 과정에서의 파라미터 값 변화
- 수렴한 경우: 오른쪽 그래프가 안정적인 랜덤 워크 형태

**Pair Plot**:
- 대각선: 각 파라미터의 주변 분포
- 비대각선: 파라미터 쌍 간의 결합 분포
- 파라미터 간 상관관계와 불확실성 구조 파악

## 📦 의존성

### 핵심 라이브러리

- **`pymc`**: Bayesian 통계 모델링과 MCMC 추론
  - 확률적 프로그래밍 프레임워크
  - Gaussian Process 모델링 지원
  - 다양한 MCMC 샘플러 제공

- **`numpy`**: 수치 연산과 다차원 배열 처리
  - 기본적인 수학 연산 및 선형대수
  - 데이터 생성, 변환, 통계 계산

- **`arviz`**: Bayesian 분석 결과 요약 및 시각화
  - MCMC 체인 진단 및 수렴성 검사
  - Trace plot, posterior plot 등 시각화 도구

- **`pandas`**: 표 형식 데이터 처리
  - CSV 파일 읽기/쓰기
  - 데이터프레임 조작 및 전처리

- **`matplotlib`**: 데이터 시각화
  - 2D 플롯 생성 및 커스터마이징
  - 결과 시각화 및 분석 도표 작성

- **`pytensor`**: 텐서 연산 지원 (PyMC 백엔드)
  - 심볼릭 텐서 연산 라이브러리
  - 자동 미분 및 그래디언트 계산
  - PyMC 내부 연산에 사용

### 추가 라이브러리

- **`jupyter`**: 노트북 환경
- **`numpyro`**: NUTS 샘플러 백엔드 (빠른 성능)

## 🔧 고급 설정

### 다중 체인 샘플링 (권장)

단일 체인보다는 여러 체인을 동시에 실행하는 것이 수렴성 진단에 유리하다:

```python
trace = pm.sample(
    draws=1000,
    tune=1000,
    chains=4,        # 4개 체인으로 병렬 샘플링
    cores=4,         # 4개 코어 사용
    target_accept=0.8
)
```

**장점:**
- $\hat{R}$ (R-hat) 통계량으로 수렴성 진단 가능
- Effective Sample Size (ESS) 정확한 계산
- 수렴 실패 조기 발견

### 수렴성 진단

**주요 지표:**
1. **Trace plot**: 시각적으로 수렴 여부 확인
2. **$\hat{R}$ (R-hat)**: 1.01 이하면 수렴 (chain이 2개 이상일 때)
3. **ESS (Effective Sample Size)**: 독립적인 샘플 개수 (클수록 좋음)
4. **Autocorrelation**: 낮을수록 독립적인 샘플

## 💡 팁과 주의사항

### 초심자를 위한 팁

1. **데이터 정규화 필수**: GP 모델링에서는 모든 변수를 동일한 스케일로 변환하는 것이 중요하다.

2. **워밍업 단계의 중요성**: `tune` 파라미터를 충분히 크게 설정해야 샘플러가 최적의 파라미터를 찾는다.

3. **단일 체인의 한계**: 실제 분석에서는 최소 2개 이상의 체인을 사용해야 수렴성 진단이 가능하다.

4. **결과 해석 시 역변환**: 정규화된 파라미터를 원래 물리적 단위로 역변환해야 의미 있는 해석이 가능하다.

### 주의사항

1. **수렴성 확인**: Trace plot에서 파라미터 값이 안정적으로 변화하는지 확인한다.

2. **샘플 크기**: 복잡한 모델일수록 더 많은 샘플(`draws`)이 필요하다.

3. **사전 분포 선택**: 물리적으로 타당한 범위를 가지는 사전 분포를 선택한다.

4. **계산 시간**: GP 모델은 데이터 크기의 세제곱에 비례하여 계산 시간이 증가한다.

## 📚 참고 자료

- [PyMC 공식 문서](https://www.pymc.io/)
- [PyMC GP Marginal Implementation](https://www.pymc.io/projects/examples/en/latest/gaussian_processes/GP-Marginal.html)
- [ArviZ 문서](https://python.arviz.org/)
- [uv 공식 문서](https://docs.astral.sh/uv/)
- Kennedy, M. C., & O'Hagan, A. (2001). Bayesian calibration of computer models. *Journal of the Royal Statistical Society: Series B*, 63(3), 425-464.
