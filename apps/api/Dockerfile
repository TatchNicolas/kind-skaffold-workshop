FROM python:3.10 as builder
WORKDIR /opt

RUN pip install poetry

COPY pyproject.toml pyproject.toml
COPY poetry.lock poetry.lock

RUN poetry config virtualenvs.create false
RUN poetry install

RUN poetry export -f requirements.txt --output requirements.txt
RUN poetry export --dev -f requirements.txt --output dev-requirements.txt

# 必要なアプリケーションコード/パッケージ類だけを抜く
FROM python:3.10 as app
WORKDIR /opt
COPY --from=builder /usr/local/lib/python3.10/site-packages/ /usr/local/lib/python3.10/site-packages/ 
COPY app/ app/

# テストに必要なものを入れる
FROM builder as app_with_test
WORKDIR /opt
COPY --from=builder /opt/dev-requirements.txt /opt/dev-requirements.txt
COPY --from=app /opt/app /opt/app/ 
RUN pip install -r dev-requirements.txt
COPY test/ test/
