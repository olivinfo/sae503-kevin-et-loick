FROM python:3.13-alpine
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
RUN pip install gunicorn
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "app:app"]