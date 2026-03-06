FROM maven AS build
WORKDIR /app
COPY . .
RUN mvn package
RUN mv target/*.jar App.jar

FROM eclipse-temurin:25-alpine
WORKDIR /package
COPY --from=build /app/App.jar .
ENTRYPOINT ["java", "-jar", "App.jar"]
