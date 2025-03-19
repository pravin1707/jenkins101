FROM jenkins/agent:alpine-jdk21
USER root
RUN apk add --no-cache python3 py3-pip
WORKDIR /myapp
COPY ../myapp/requirements.txt /app/
RUN python3 -m venv /app/venv
RUN /app/venv/bin/pip install --upgrade pip
RUN /app/venv/bin/pip install -r /app/requirements.txt
COPY ../myapp/ /app/
USER jenkins
CMD ["/app/venv/bin/python", "app.py"]

# RUN apk add python3
# RUN apk add py3-pip
# USER jenkins