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
	age: int = Field(gt=0, lt=150) # Field: 추가적인 규칙이나 설명을 붙이고 싶을 때 사용
	email: str | None = None # optional field

	model_config = ConfigDict(extra="forbid") # 정의하지 않은 필드가 들어오면.. ignore(무시), allow(허용), forbid(에러)

# data 생성 및 검증
user = User(id="123", name="Alice", age=25)
print(user.id)            # 123 ("123" -> 123으로 변환 됨)
user.model_json_schema    # json Schema로 변환
```
## Hook
복잡한 논리 검증 및 형태 가공 등을 위해 사용
```python
# model_validatior 예시
class ContinuousEvent(BaseModel):
	"""지속성 사운드 이벤트 정보를 담는 모델 - start_time과 end_time으로 표현"""
	type: Literal["continuous"]
	start_time: float = Field(ge=0)
	end_time: float = Field(ge=0)

	model_config = ConfigDict(extra="forbid")

	# end_time이 start_time보다 커야 한다는 제약 조건을 검증하는 모델 검증기.
	@model_validator(mode="after") # mode: 검증 시점을 결정 (after -> 모델이 생성된 이후)
	def check_order(self) -> "ContinuousEvent":
		if self.end_time <= self.start_time:
			raise ValueError("ContinuousEvent end_time must be greater than start_time")
	return self # 모델 반환


# field_validator 예시
class LocalPreprocessingResult(BaseModel):
	"""stage 01 출력 계약: 로컬 전처리 단계의 결과를 담는 모델"""
	video_metadata: VideoMetadata
	cuts: List[Cut]
	video_path: str = Field(min_length=1)
	video_mime_type: str = Field(min_length=1)
  
	model_config = ConfigDict(extra="forbid")

	# cuts 리스트가 비어있지 않은지 검증하는 필드 검증기.
	@field_validator("cuts")
	def non_empty_cuts(cls, value: List[Cut]) -> List[Cut]:
		if not value:
			raise ValueError("LocalPreprocessingResult must contain at least one cut")
		return value

# model_serializer 예시
class Entity(BaseModel):
	"""Agent A 단계에서 추출된 Entity 정보를 담는 모델"""
	
	id: str = Field(min_length=1)
	label: str = Field(min_length=1)
	children: List[Entity_Child] = Field(default_factory=list)
	model_config = ConfigDict(extra="forbid")

	@model_serializer(mode="wrap")
	def serialize_model(self, serializer):
		payload = serializer(self)
		if not self.children:
			payload.pop("children", None) # children이 비어있으면 직렬화 결과에서 제거
		return payload
```
## Method
```python
- model_validate(...)
    - dict -> Pydantic 모델 객체
- model_dump()
    - Pydantic 모델 객체 -> dict
```
