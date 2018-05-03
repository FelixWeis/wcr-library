FROM python:3-stretch

RUN pip install --no-cache-dir jupyter requests numpy pandas

WORKDIR /root

VOLUME /root

CMD ["jupyter", "notebook", "--allow-root", "--ip=0.0.0.0"]

