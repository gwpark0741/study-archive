구글 통합 [[SDK]]

```bash
# SDK 설치
uv add google-genai
```
```python
from google import genai # goole package 내, genai 모듈을 가져옴
from google.genai import type # types 모듈

# 클라이언트 생성
# Google AI server 통신을 위한 객체 : API key, 서버주소 네트워크 설정 등을 포함 
# 사용자 <-> 클라이언트 <-> Google AI server
client = genai.Client(api_key="your_key")

# 요청
response = client.models.generate_content(
	model = "gemini-2.5-pro", # 사용할 모델 이름
	contents = "이 사진에 나오는 사람의 표정을 설명", # 질문, 요청
 	config = types.GenerateContentConfig(    # 모델 동작 설명
		temparature = 0.2,
		max_output_token = 1000,
		stop_sequence=["[End]"],
		system_instruction=SYSTEM_PROMPT)                  
	)
)

print(response.text)
```
