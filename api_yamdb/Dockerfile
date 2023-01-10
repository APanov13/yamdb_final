FROM python:3.7-slim
WORKDIR /app
# COPY ./api_yamdb/requirements.txt .
COPY requirements.txt /app
# COPY ./api_yamdb/requirements.txt /app

RUN ls

RUN pip3 install -r requirements.txt --no-cache-dir
COPY . .
# COPY . ./app
CMD ["gunicorn", "api_yamdb.wsgi:application", "--bind", "0:8000" ]
# CMD ["python3", "manage.py", "runserver", "0:8000"] 


# FROM python:3.7
# RUN mkdir /app
# COPY requirements.txt /app
# RUN python3 -m pip install --upgrade pip
# RUN pip3 install -r /app/requirements.txt --no-cache-dir
# COPY api_yamdb/ /app
# WORKDIR /app
# CMD ["gunicorn", "api_yamdb.wsgi:application", "--bind", "0:8000"]

