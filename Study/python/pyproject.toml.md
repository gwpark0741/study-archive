## pyproject.toml 이란?
- python 공식 권장 설정파일
- 패키징 도구(setuptools, poetry 등)가 이 파일을 읽고 설치/빌드를 수행
- 과거 `setup.py`, `requirement.txt` 등 따로 필요했던 설정 파일을 하나로 합쳐서 관리 가능
## 담기는 정보
- 프로젝트 기본 정보: name, version, description
- 파이썬 버전: 최조 요구 버전 지정 가능 (>= 3.11)
- 의존성: 실행에 필요한 외부 패키지 목록
- 빌드 도구 설정: 어떤 도구를 사용할지(setuptools, poetry 등)
```toml
# 빌드 도구 설정 : 이 프로젝트를 어떤 도구로 빌드할지 결정
[build-system]
requires = ["setuptools>=68", "wheel"] 
build-backend = "setuptools.build_meta"

# 프로젝트 메타데이터 
[project] 
name = "mypkg" 
version = "0.1.0" 
description = "Demo package" 
requires-python = ">=3.11" 
# 의존성 관리
dependencies = [
    "pydantic>=2.7.0",
    "instructor>=1.3.0",
    "openai",
]
```

| **구분**    | **과거 방식 (Legacy)**                                 | **현대적 방식 (pyproject.toml)** |
| --------- | -------------------------------------------------- | --------------------------- |
| **설정 파일** | `setup.py`, `setup.cfg`, `requirements.txt` 등 여러 개 | `pyproject.toml` 하나로 통합     |
| **가독성**   | 파이썬 코드 기반이라 읽기 복잡함                                 | `TOML` 형식으로 데이터가 정돈         |
| **표준화**   | 도구마다 설정 방식이 다름                                     | 어떤 빌드 도구를 쓰든 공통 규격을 따름      |
| **안전성**   | 설치 시 임의의 코드가 실행될 수 있음 (setup.py)                   | 단순 설정 파일이라 실행 위험이 없음        |
