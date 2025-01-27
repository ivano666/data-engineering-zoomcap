FROM python:3.12.8

RUN pip install pandas

WORKDIR /opt/app
COPY pipeline.py pipeline.py

ENTRYPOINT ["python", "pipeline.py"]

# uncomment to answer to question 1 in the homework
# ENTRYPOINT [ "bash" ] 