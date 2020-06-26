#This container contains your model and any helper scripts specific to your model.
FROM gcr.io/kubeflow-images-public/tensorflow-2.1.0-notebook-gpu:1.0.0

USER root 

# copy requirements
ADD requirements.txt /opt/requirements.txt
# install requirements
ADD requirements.txt /opt/requirements.txt
RUN pip install -r /opt/requirements.txt

# copy model
ADD model.py /opt/model.py
ADD storage.py /opt/storage.py
RUN chmod +x /opt/model.py
RUN chmod +x /opt/storage.py

ENTRYPOINT ["python"]
CMD ["/opt/model.py"]