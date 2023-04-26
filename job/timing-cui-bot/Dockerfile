FROM openjdk:8-alpine as builder

MAINTAINER lazysheep

ARG PATH=/home/server/job/timing-cui-bot
ARG TIMING_CUI_BOT=${PATH}
ARG TIMING_NAME=timing-cui-bot
ARG JAR_FILE=/*.jar

WORKDIR /home/server/job/timing-cui-bot

ADD ${JAR_FILE} /timing-cui-bot.jar

#COPY /timing-cui-bot-*.jar ${TIMING_CUI_BOT}/bin/timing-cui-bot.jar

RUN java -Djarmode=layertools -jar /${TIMING_NAME}.jar extract


FROM openjdk:8-alpine as runtime

MAINTAINER lazysheep

WORKDIR /home/server/job/timing-cui-bot

ENV TZ="Asia/Shanghai"
#设置JVM参数
ENV JAVA_OPTIONS "-Xms256m -Xmx256m -XX:MetaspaceSize=56m"
ENV JAVA_OPTS ""

COPY --from=builder /home/server/job/timing-cui-bot/dependencies/ ./
COPY --from=builder /home/server/job/timing-cui-bot/spring-boot-loader/ ./
COPY --from=builder /home/server/job/timing-cui-bot/snapshot-dependencies/ ./
COPY --from=builder /home/server/job/timing-cui-bot/application/ ./

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

EXPOSE 9010

ENTRYPOINT ["sh", "-c", "exec java $JAVA_OPTIONS ${JAVA_OPTS} org.springframework.boot.loader.JarLauncher"]