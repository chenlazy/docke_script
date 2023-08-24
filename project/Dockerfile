FROM openjdk:8 as builder

MAINTAINER chendl@biyouxinli.com

ARG PATH=/Users/lazysheep/data/docker/lxcloud-vhr
ARG LXCLOUD_VHR_PATH=${PATH}
ARG APPLICATION_NAME=lxcloud-vhr
ARG JAR_FILE=target/*.jar

WORKDIR ${PATH}

#ADD ${JAR_FILE} ${APPLICATION_NAME}.jar

ADD ${APPLICATION_NAME}.jar ${APPLICATION_NAME}.jar

RUN java -Djarmode=layertools -jar ${APPLICATION_NAME}.jar extract

FROM openjdk:8-alpine as runtime

MAINTAINER chendl@biyouxinli.com

ARG KEYWORD=${APPLICATION_NAME}
ARG SKYWALKING_DIR="/opt/skywalking-es7"
ARG LOG_DIR=/opt/logs/$KEYWORD

WORKDIR ${PATH}

ENV TZ="Asia/Shanghai"
#设置JVM参数
ENV JAVA_OPTIONS "server -Xms1024M -Xmx1024M -Xmn512M \
 -XX:MetaspaceSize=128M -XX:MaxMetaspaceSize=512M \
  -Xss512k -XX:ParallelGCThreads=20 -XX:+UseConcMarkSweepGC \
   -XX:+PrintGCDetails -XX:+PrintGCDateStamps \
    -Djava.io.tmpdir=/tmp/ -XX:-OmitStackTraceInFastThrow \
     -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=$LOG_DIR.hprof -XX:-UseLargePages \
      -Xloggc:$LOG_DIR-gc.log -verbose:gc -XX:+PrintGCDetails -XX:+PrintGCDateStamps \
       -XX:+PrintGCTimeStamps -XX:+UseGCLogFileRotation \
        -XX:NumberOfGCLogFiles=10 -XX:GCLogFileSize=100M"
ENV JAVA_OPTS "-javaagent:$SKYWALKING_DIR/$KEYWORD/skywalking-agent.jar \
-Dskywalking.agent.service_name=$KEYWORD"

COPY --from=builder /home/server/job/timing-cui-bot/dependencies/ ./
COPY --from=builder /home/server/job/timing-cui-bot/spring-boot-loader/ ./
COPY --from=builder /home/server/job/timing-cui-bot/snapshot-dependencies/ ./
COPY --from=builder /home/server/job/timing-cui-bot/application/ ./

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

EXPOSE 9040
EXPOSE 20098

ENTRYPOINT ["sh", "-c", "exec java $JAVA_OPTIONS ${JAVA_OPTS} org.springframework.boot.loader.JarLauncher"]