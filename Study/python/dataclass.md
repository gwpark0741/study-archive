```python
# dataclass 없이 데이터를 담는 class를 만드는 방식
# 코드가 길어지고, 필드 변경 및 추가 시 여러 군데 수정필요
class PipelineConfig:
    def __init__(self, mode, model, output_dir):
        self.mode = mode
        self.model = model
        self.output_dir = output_dir

    def __repr__(self):  # print() 했을 때 보기 좋게
        return f"PipelineConfig(mode={self.mode}, model={self.model})"

    def __eq__(self, other):  # == 비교할 때
        return self.mode == other.mode and self.model == other.model


# dataclass
# @dataclass 데코레이터 -> 위 __init__, __repr__, __eq__ 를 자동으로 만들어줌 (위아래가 완전히 동일한 코드임)
from dataclass import dataclass

@dataclass(frozen=True)
class PipelineConfig:
	mode: str
	model: str
	output_dir: str
```