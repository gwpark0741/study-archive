```python
from typing import TypedDict, Optional

# 일반 dict: 키/값 타입 모름 
state = {"video_path": "test.mp4", "duration": 10.0} 

# TypedDict: 키/값 타입 명시한 dictionary -> 내부 관리 데이터/상태(LangGraph State)
# 타입이 달라도 그냥 통과, 런타임에는 그냥 dict VS. BaseModel: 값 검증, 자동 변환, ValidationError 발생 (느림) -> 외부 요청/입력 (신뢰 불가)
# IDE 자동완성과 정적 분석(mypy)을 위함
class PipelineState(TypedDict): 
	video_path: str 
	duration: float
	
# Optional	
uploaded_file_uri: Optional[str]
# 위와 동일
uploaded_file_uri: str | None  # Python 3.10+ 문법
```