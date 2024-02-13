FROM azul/zulu-openjdk:21 as base
  
# Configure the build environment
FROM base as build
RUN wget https://mirrors.estointernet.in/apache/maven/maven-3/3.8.5/binaries/apache-maven-3.8.5-bin.tar.gz
RUN tar -xvf apache-maven-3.8.5-bin.tar.gz
RUN sudo mv apache-maven-3.8.5 /opt/


RUN M2_HOME='/opt/apache-maven-3.8.5'
RUN PATH="$M2_HOME/bin:$PATH"
RUN export PATH

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

    
# Set runtime interface client as default command for the container runtime
ENTRYPOINT [ "/usr/bin/java", "-cp", "./*", "com.amazonaws.services.lambda.runtime.api.client.AWSLambda" ]

# Set the CMD to your handler (could also be done as a parameter override outside of the Dockerfile)
# CMD [ "software.amazonaws.example.product.handler.GetProductByIdHandler::handleRequest" ]  