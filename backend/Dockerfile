FROM maven AS build
WORKDIR /app
COPY . .
RUN mvn package
RUN mv /app/target/*.jar /app/target/App.jar


FROM openjdk:17-jdk-slim
WORKDIR /app

COPY --from=build /app/target/App.jar .
EXPOSE 9090
CMD ["java","-jar","App.jar"]

