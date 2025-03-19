FROM jenkins/agent:alpine-jdk21
USER root

# Install Python and pip
RUN apk add --no-cache python3 py3-pip

# Create and activate a virtual environment
WORKDIR /app
RUN python3 -m venv /app/venv
RUN /app/venv/bin/pip install --upgrade pip

# Copy application files and install dependencies
COPY . /app
RUN /app/venv/bin/pip install -r requirements.txt

# RUN apk add python3
# RUN apk add py3-pip
USER jenkins

# Run the app using the virtual environment
CMD ["/app/venv/bin/python", "app.py"]