FROM openjdk:8 AS BUILD_IMAGE 
RUN apt update && apt install maven -y
RUN git clone -b vp-docker https://github.com/dazz-matt/mb-demo-app-1.git
RUN cd mb-demo-app-1 && mvn install

FROM tomcat:8-jre11

RUN rm -rf /usr/local/tomcat/webapps/*

COPY --from=BUILD_IMAGE mb-demo-app-1/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
CMD ["catalina.sh", "run"]
