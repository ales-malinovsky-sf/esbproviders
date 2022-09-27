ARG  BASE_IMAGE=harbor-infra.rfosp.local/library/springboot:1-jdk17-2022.06.24-0


FROM ${BASE_IMAGE} as builder
COPY ["image-data/", "/"]

WORKDIR /home/java/app
# must exist exactly one *.jar
RUN java -Djarmode=layertools -jar $(ls *.jar) extract


FROM ${BASE_IMAGE}
COPY ["image-data/home/java/bin/jar-launcher", "/home/java/bin/"]

WORKDIR /home/java/app
COPY --from=builder /home/java/app/dependencies/ ./
COPY --from=builder /home/java/app/spring-boot-loader/ ./
COPY --from=builder /home/java/app/snapshot-dependencies/ ./
COPY --from=builder /home/java/app/application/ ./

ENTRYPOINT ["/home/java/bin/jar-launcher"]
EXPOSE 8080
