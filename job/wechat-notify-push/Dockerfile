FROM openjdk:8-alpine as builder

MAINTAINER derling-chen

ARG APPLICATION_NAME=wechat-notify-push
ARG PATH=/home/server/job/${APPLICATION_NAME}
ARG JAR_FILE=/*.jar

ADD ${JAR_FILE} /home/server/job/wechat-notify-push/wechat-notify-push.jar

WORKDIR /home/server/job/wechat-notify-push

RUN java -Djarmode=layertools -jar /home/server/job/wechat-notify-push/wechat-notify-push.jar extract

FROM openjdk:8-alpine as runtime

MAINTAINER derling-chen

WORKDIR /home/server/job/wechat-notify-push

ENV TZ="Asia/Shanghai"
#设置JVM参数
ENV JAVA_OPTIONS "-Xms256m -Xmx512m -XX:MetaspaceSize=56m"
ENV JAVA_OPTS ""

COPY --from=builder /home/server/job/wechat-notify-push/dependencies/ ./
COPY --from=builder /home/server/job/wechat-notify-push/spring-boot-loader/ ./
COPY --from=builder /home/server/job/wechat-notify-push/snapshot-dependencies/ ./
COPY --from=builder /home/server/job/wechat-notify-push/application/ ./

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

EXPOSE 8099

ENTRYPOINT ["sh", "-c", "exec java $JAVA_OPTIONS ${JAVA_OPTS} org.springframework.boot.loader.JarLauncher"]