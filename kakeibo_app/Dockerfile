# 公式からpython3.7 on alpine linuxイメージをpull
FROM python:3.7-alpine

# 作業ディレクトリを設定
WORKDIR /usr/src/app

# 環境変数を設定
# Pythonがpyc filesとdiscへ書き込むことを防ぐ
ENV PYTHONDONTWRITEBYTECODE 1
# Pythonが標準入出力をバッファリングすることを防ぐ
ENV PYTHONUNBUFFERED 1

# venv環境の構築
RUN pip install --upgrade pip \
    && python -m venv venv \
    && source venv/bin/activate \

# requirements.txtをコピー
COPY ./requirements.txt /usr/src/app/requirements.txt

# Django等をインストール
RUN pip install -r requirements.txt
# psycopg2のインストール
RUN apk update \
    && apk add --virtual build-deps gcc python3-dev musl-dev \
    && apk add postgresql-dev \
    && pip install psycopg2 \
    && apk del build-deps


# entrypoint.shをコピー
COPY ./entrypoint.sh /usr/src/app/entrypoint.sh

# ホストのカレントディレクトリ（現在はappディレクトリ）を作業ディレクトリにコピー
COPY . /usr/src/app/

# entrypoint.shを実行
ENTRYPOINT ["/usr/src/app/entrypoint.sh"]
