# 개발환경
- python
- uv
- openai-api


# uv 세팅
```
uv init your-project-name
```


# uv 사용법

## pyproject.toml 기준으로 dependency 동기화
```
uv sync
```

## dependency 추가
- for project
```
uv add firecrawl-py
```

## 사용할 가상환경 세팅
- .venv

## 파일 실행 
```
uv run main.py
```

## extention
- python
- jupyter

## .ipynb
- jupyter 실행파일

## dependency group
- for developers
```
#  jupyter notebook 실행용
uv add ipykernel --dev
```

