#
# Build stage
#
FROM maven:3.8.4-openjdk-17-slim AS build

COPY lib ./submodules/comment-analysis-neon/lib
COPY src ./submodules/comment-analysis-neon/src
COPY ./submodules/comment-analysis-neon/pom.xml .
COPY ./submodules/comment-analysis-neon/lib/org/neon/neon/0.0.3/neon-0.0.3.jar neonlib
RUN mvn validate
RUN mvn install:install-file -DgroupId=org.neon -DartifactId=neon -Dversion=0.0.3 -Dfile=neonlib -Dpackaging=jar -DgeneratePom=false -DcreateChecksum=true
RUN mvn install

COPY lib ./lib
COPY src ./src
COPY pom.xml .
RUN mvn validate
RUN mvn -f ./pom.xml clean package

#
# Package stage
#
FROM openjdk:latest
COPY --from=build ./target/connector-api-0.0.1-SNAPSHOT.jar /usr/local/lib/connector-api.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","/usr/local/lib/connector-api.jar"]
