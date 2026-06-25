FROM maven as stage1
WORKDIR /backend
COPY . .
RUN mvn package
RUN mv target/*.jar App.jar

FROM eclipse-temurin:26-jdk-alpine
WORKDIR /app
COPY --from=stage1 /backend/App.jar .
CMD ["java", "-jar", "App.jar"]
