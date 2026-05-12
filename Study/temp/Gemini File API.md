`file_uri`
- 업로드된 파일을 가리키는 URI 형태 식별자
- 보통 모델 입력이나 참조용 주소 개념
- 예: “이 파일 리소스는 서버의 어디에 있다”는 정보
- → 리소스 소비용(위치/참조) 정보 : name 노출하지 않고 사용만 할 수 있도록

`file_name`
- File API 내부에서 파일을 조회할 때 쓰는 직접 식별자
- 보통 client.files.get(name=...)에서 필요한 값
- 예: 서버가 부여한 파일 object의 고유 이름
- → 리소스 관리용(삭제, 조회) primary key

```python
# 1. file_name 사용 사례 (관리자 모드)
# 서버에 "내가 준 ID가 'files/abc-123'인 녀석 상태 좀 알려줘"라고 요청
file_info = client.files.get(name=state["file_name"]) 

# file_name으로 조회된 파일 객체(file_ref) 제공
response = client.models.generate_content(
	model=model_name,
	contents=[video_ref, user_prompt],
	config=types.GenerateContentConfig(
		system_instruction=system_prompt,
		temperature=temperature,
		seed=seed,
		response_mime_type="application/json",
	)
)

# 2. file_uri 사용 사례 (사용자/모델 모드)
# 모델에게 "이 주소(https://...)에 있는 영상 분석해줘"라고 요청
response = client.models.generate_content( # 대용량 데이터 스트리밍을 위해 URI 프로토콜 사용하도록 설계됨
    model="gemini-2.0-flash",
    contents=[
        types.Part.from_uri(file_uri=state["file_uri"], mime_type="video/mp4"),
        "영상 내용을 요약해줘."
    ]
)
```

| **구분**     | **from_uri (URI 문자열 전달)** | **video_ref (객체 직접 전달)**     |
| ---------- | ------------------------- | ---------------------------- |
| **데이터 형태** | 단순 문자열 (String)           | SDK 내부 클래스 인스턴스 (Object)     |
| **사전 절차**  | 없음 (주소만 알면 즉시 호출)         | `client.files.get()` 호출이 선행됨 |
| **안정성**    | 파일이 죽었는지 살았는지 모름          | 서버 상태를 확인한 후이므로 매우 안전        |
| **추천 상황**  | 주소만 넘겨받은 하위 모듈에서 실행 시     | 전처리(Polling) 직후 바로 추론할 시     |
