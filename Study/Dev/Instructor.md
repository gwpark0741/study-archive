Pydantic을 외부 LLM에 연결해주는 라이브러리 [[Pydantic - BaseModel]]
LLM은 기본적으로 텍스트를 출력하지만, Instructor를 사용하여 LLM 응답을 즉시 Pydantic 모델로 받을 수 있음
구조화된 출력을 얻기 위한 표준 도구가 됨
```
uv add instructor
```
## 주요 기능
1. 자동 유효성 검사 및 재시도
2. 다양한 모델 지원: `liteLLM`과 결함으로 OpenAI뿐 아니라 Anthropic, Google Gemini 등 모든 모델에서 Pydantic 문법으로 정형화된 응답을 뽑아낼 수 있음 
```python
import instructor
from openai import OpenAI
from pydantic import BaseModel

class Userinfo(BaseModel):
	name: str
	age: int
	email: str
	
client = instructor.from_openai(OpenAI()):

##### 자동 유효성 검사 및 재시도 #####
# AI가 email을 빠뜨렸을 때 
# ① AI 응답: {"name": "Alice", "age": 25} ← email 없음 
# ② Instructor: "email 필드가 없어, 다시 해!" 
# ③ AI 응답: {"name": "Alice", "age": 25, "email": "alice@gmail.com"} ← 완성
user_data = client.chat.completations.create(
	model="gpt-4o",
	response_model=UserInfo,
	massage=[
		{"role": "user", "content": "안녕하세요, 제 이름은 김파이이고 25살입니다. 메일 주소는 py@example.com 이에요."}
	]
)

# 결과는 문자열이 아닌 완벽한 파이썬 객체
print(f"이름: {user_data.name}") 
print(f"나이: {user_data.age}") 
print(f"이메일: {user_data.email}")
```

```python
import litellm
import instructor
from pydantic import BaseModel

class Summary(BaseModel):
    title: str
    points: list[str]

# 모든 모델에서 동일한 문법
client = instructor.from_litellm(litellm.completion)

# OpenAI
result = client.chat.completions.create(
    model="gpt-4o",
    response_model=Summary,
    messages=[{"role": "user", "content": "뉴스 요약해줘"}]
)

# Anthropic - 코드 동일, model만 바꿈
result = client.chat.completions.create(
    model="claude-sonnet-4.6",
    response_model=Summary,
    messages=[{"role": "user", "content": "뉴스 요약해줘"}]
)

# Gemini - 코드 동일, model만 바꿈
result = client.chat.completions.create(
    model="gemini/gemini-2.0-flash",
    response_model=Summary,
    messages=[{"role": "user", "content": "뉴스 요약해줘"}]
)

# 결과는 항상 Summary 객체로 동일
print(result.title)   # str 보장
print(result.points)  # list[str] 보장
```