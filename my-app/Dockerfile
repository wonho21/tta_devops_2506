# First stage: Build environment
FROM python:3.12-slim AS builder
WORKDIR /app

# Poetry 설치 및 종속성 빌드
RUN pip install poetry
COPY pyproject.toml poetry.lock ./

# 패키지 빌드
COPY . .
RUN poetry build -f wheel

# Second stage: Runtime environment
FROM python:3.12-slim
WORKDIR /app

# 빌드된 .whl 파일을 복사하고 설치
COPY --from=builder /app/dist/*.whl .
RUN pip install --no-cache-dir my_app-*.whl

# FastAPI 서버 실행 - ENTRYPOINT와 CMD 분리
# ENTRYPOINT는 container 실행시 반영되는 명령어. 이후 변경 불가
# CMD는 container 실행시 반영되는 명령어. 하지만, 실행시 인자값으로 변경 가능
ENTRYPOINT ["uvicorn", "my_app.main:app"]
CMD ["--host", "0.0.0.0", "--port", "8000"]