FROM azul/zulu-openjdk:21-jdk-crac-latest as base
  
# Configure the build environment
FROM base as build
# Install maven
RUN apt-get update \
 && DEBIAN_FRONTEND=noninteractive \
    apt-get install -y maven sudo \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*  
WORKDIR /src

# Cache and copy dependencies
ADD pom.xml .
RUN mvn dependency:go-offline dependency:copy-dependencies

# Compile the function
ADD . .
RUN mvn package 

# Copy the function artifact and dependencies onto a clean base
FROM base
WORKDIR /function

COPY --from=build /src/target/dependency/*.jar ./
COPY --from=build /src/target/*.jar ./

RUN  sudo chown root:root $JAVA_HOME/lib/criu
RUN  sudo chmod u+s $JAVA_HOME/lib/criu
    
# Set runtime interface client as default command for the container runtime
#ENTRYPOINT [ "/usr/bin/java", "-cp", "./*", "com.amazonaws.services.lambda.runtime.api.client.AWSLambda" ]

ENTRYPOINT $JAVA_HOME/bin/java -cp "./*" com.amazonaws.services.lambda.runtime.api.client.AWSLambda org.springframework.cloud.function.adapter.aws.FunctionInvoker::handleRequest
# Set the CMD to your handler (could also be done as a parameter override outside of the Dockerfile)
#CMD [ "org.springframework.cloud.function.adapter.aws.FunctionInvoker::handleRequest" ]  