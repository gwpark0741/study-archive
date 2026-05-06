## 찾게된 경로
pyproject.toml을 읽던 중 아래 와 같은 의존성이 존재
`scenedetect[opencv] >= 0.6.7.1`
- `scenedetect` : PySceneDetect 라이브러리
- `opencv` : 컴퓨터비전 라이브러리
- `[opencv]` : scenedetect가 opencv가 필요하니 함께 설치해라, scenedetect 내부에 적힌 가이드를 따라 적합한 버전의 OpenCV를 설치해라