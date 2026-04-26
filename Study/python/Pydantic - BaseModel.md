# Pydantic
python 데이터 검증 라이브러리
```
uv add pydantic
```

python은 기본적으로 타입을 강제하지 않음
```python
def greet(name: str):
	print(name)
	
greet(123) # 오류없이 실행 됨
```
→ Pydantic은 타입 힌트 (type hint)를 사용하여 데이터를 검증
## 핵심 기능
1. 데이터 검증: 입력 데이터가 지정한 타입과 일치하는지 확인
2. 타입 변환: `int` 타입 필드에 문자열 `"123"`이 들어오면 자동으로 숫자 `123`로 변환
3. JSON 스키마 생성: 정의한 모델 기반 JSON 스키마 생성 → API 용 문서화 용이
# BaseModel
pydantic 라이브러리에서 데이터를 정의하는 기본적인 class
class를 정의하고 이를 상속받아 사용자가 원하는 스키마를 정의
```python
from pydantic import Basemodel, Field

class User(BaseModel):
	id: int
	name: str
	age: int = Field(gt=0, lt=150) # 0 <= age <= 150
	email: str | None = None # optional field

# data 생성 및 검증
user = User(id="123", name="Alice", age=25)
print(user.id)            # 123 ("123" -> 123으로 변환 됨)
user.model_json_schema    # json Schema로 변환
```
