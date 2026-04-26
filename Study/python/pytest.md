널리 사용되는 테스팅 프레임워크
## 주요 기능
1. 간결한 테스트 코드 작성
2. 함수 단위 자동 테스트
	1. 현재 디렉토리 - 하위 디렉토리에서 테스트를 찾음
	2. test_\*.py \*\_test.py 파일만 검색 → 파일 내부에서 test_로 시작하는 함수, Test class 내부 test_로 시작하는 메서드
3. 디버깅 용이
	1. 실제 값 vs 기대값 비교 
	2. 함수 호출결과 출력
4. fixture 
### sample code
```python
import pytest

def add(a, b): # 테스트할 함수
	return a + b
	
def test_add(a, b):
	assert add(2, 3) == 5
	assert add(4, 5) == 9
	
@pytest_fixture
def sample_data(): # 테스트 전/후에 실행할 코드, 테스트 환경 설정/정리 도와줌
	return [1, 2, 3, 4, 5]
	
def test_with_fixture(sample_data):
	assert sum(sample_data) == 15
```