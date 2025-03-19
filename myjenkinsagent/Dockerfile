FROM jenkins/agent:alpine-jdk21
USER root
RUN apk add --no-cache python3 py3-pip
WORKDIR /app
RUN python3 -m venv /app/venv
RUN /app/venv/bin/pip install --upgrade pip
COPY . /app
RUN /app/venv/bin/pip install -r /app/requirements.txt
USER jenkins
CMD ["/app/venv/bin/python", "app.py"]

# RUN apk add python3
# RUN apk add py3-pip
# USER jenkins