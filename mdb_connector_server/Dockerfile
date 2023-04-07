FROM eclipse-temurin:17-jdk-alpine as build
ARG REPO=mdb_connector_server
WORKDIR /app

# copy necessary files into container
COPY ${REPO}/mvnw .
COPY ${REPO}/.mvn .mvn
COPY ${REPO}/pom.xml .
COPY ${REPO}/src src

# build
RUN ./mvnw install -DskipTests
RUN mkdir -p target/dependency && (cd target/dependency; jar -xf ../*.jar)

# run app
FROM eclipse-temurin:17-jdk-alpine
VOLUME /tmp
ARG DEPENDENCY=/app/target/dependency
COPY --from=build ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY --from=build ${DEPENDENCY}/META-INF /app/META-INF
COPY --from=build ${DEPENDENCY}/BOOT-INF/classes /app
ENTRYPOINT ["java","-cp","app:app/lib/*","de.webenm.mdbconnector.MdbconnectorApplication"]