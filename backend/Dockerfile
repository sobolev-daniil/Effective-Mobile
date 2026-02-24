ARG PYTHON_VERSION=3.11-alpine
FROM python:${PYTHON_VERSION}

# Prevents Python from writing pyc files.
ENV PYTHONDONTWRITEBYTECODE=1

# Keeps Python from buffering stdout and stderr.
ENV PYTHONUNBUFFERED=1

WORKDIR /app

# Create a non-privileged user
RUN addgroup -g 1000 -S appgroup && adduser -u 1000 -S appuser -G appgroup

# Copy the source code
COPY app.py .

# Switch to non-privileged user
USER appuser

# Expose the port
EXPOSE 8080

# Run the application
CMD ["python", "app.py"]