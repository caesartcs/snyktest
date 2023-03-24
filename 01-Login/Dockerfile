
FROM python:3.12.0a5
RUN apt-get update && apt-get install -y --no-install-recommends\
 curl\
 unzip\
 && curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" &&\
 unzip awscliv2.zip &&\
 aws/install &&\
 mkdir /data_files
ARG vezecg_root="/viz_ecg_ai"
# variables that allow connection to private pypi
ARG TOKEN
ARG AWS_ACCESS_KEY_ID
ARG AWS_SECRET_ACCESS_KEY
ARG AWS_SESSION_TOKEN
COPY requirements.txt /requirements.txt
RUN apt update &&\
    python3 -m pip install --upgrade pip &&\
    python3 -m pip install --requirement /requirements.txt --extra-index-url https://aws:$TOKEN@xxx.eu-west-1.amazonaws.com/pypi/python/simple/ &&\
# To avoid missing modules, add to the common requirements.txt "srteamlit" and "plotly" packages
    pip install streamlit==1.5.1 &&\
    pip install plotly==5.7.0
ENV PYTHONPATH=$PYTHONPATH:/${vezecg_root}
EXPOSE 8501
COPY dashboards/docker/get_metadata.py /data_files
COPY dashboards/docker/commands.sh /data_files/
ENV FEATURES_PATH=/data_files/features_all_ages.csv
ENV ANNOTATIONS_PATH=/data_files/annotations
ENV MACCABI_DF_PATH=/data_files/maccabi_metadata.csv
ENV BMS_METADATA_PATH=/data_files/bms_key_list.xlsx
ENV PATIENT_HISTORY_PATH=/data_files/patient_history/diagnoses_and_drug_history.csv
ENV ECHO_DIR=/data_files/echo
ENV ECHO_METADATA_FILE=echo_metadata.csv
RUN ["chmod", "+x", "/data_files/commands.sh"]
ENTRYPOINT ["/data_files/commands.sh"]
