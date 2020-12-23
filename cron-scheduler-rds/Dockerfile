FROM python:3.6
RUN apt-get update && apt-get install -y zip
WORKDIR /lambda


ADD src/ /lambda/

RUN pip install --quiet -t /lambda -r /lambda/requirements.txt

RUN find /lambda -type d | xargs -n 1 -I {} chmod ugo+rx "{}" && \
    find /lambda -type f | xargs -n 1 -I {} chmod ugo+r "{}"

RUN python -m compileall -q /lambda

ARG ZIPFILE=cron-scheduler-rds-0.0.1.zip
RUN zip --quiet -9r /${ZIPFILE}  .

FROM scratch
ARG ZIPFILE
COPY --from=0 /${ZIPFILE} /
