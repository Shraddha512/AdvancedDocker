FROM ubuntu:14.04
MAINTAINER Shraddha Naik

RUN apt-get update  
RUN apt-get install -y socat
RUN touch temp.txt 
RUN echo "Hello this is Shraddha Naik" > temp.txt

CMD socat TCP-LISTEN:9001 SYSTEM:'cat temp.txt'
