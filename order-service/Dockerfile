FROM maven:3.8.2-jdk-11 as builder
WORKDIR /workspace/app
COPY . .
RUN mvn clean package -DskipTests

FROM mojix/serverjre:8
COPY --from=builder /workspace/app/order-web/target/order-web-1.0-SNAPSHOT.jar app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]