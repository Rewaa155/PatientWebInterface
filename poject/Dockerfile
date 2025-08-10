FROM python:3.11-slim-buster AS builder

WORKDIR /app

COPY requirements.txt .
RUN pip3 install --no-cache-dir -r requirements.txt

FROM python:3.11-slim-buster

WORKDIR /app

COPY --from=builder /usr/local/lib/python3.11/site-packages/ /usr/local/lib/python3.11/site-packages/
COPY --from=builder /usr/local/bin/pip3 /usr/local/bin/pip3
COPY --from=builder /usr/local/bin/pip /usr/local/bin/pip

COPY . .

RUN useradd --no-create-home appuser
USER appuser

EXPOSE 8000

ENTRYPOINT ["python3"]

CMD ["main.py"]

