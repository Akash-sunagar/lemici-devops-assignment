# 1. Use official Python image as base image
FROM python:3.9-slim

# 2. Set working directory inside container
WORKDIR /app

# 3. Copy requirements file first
COPY requirements.txt .

# 4. Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# 5. Copy application code
COPY app.py .

# 6. Expose port 5000
EXPOSE 5000

# 7. Run the application
CMD ["python", "app.py"]
