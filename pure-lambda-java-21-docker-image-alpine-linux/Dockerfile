FROM amazoncorretto:21-alpine 
  
# Copy function code and runtime dependencies from Maven layout
COPY target/classes ./
COPY target/dependency/* ./
    
# Set runtime interface client as default command for the container runtime
ENTRYPOINT [ "/usr/bin/java", "-cp", "./*", "com.amazonaws.services.lambda.runtime.api.client.AWSLambda" ]    

# Set the CMD to your handler (could also be done as a parameter override outside of the Dockerfile)
# CMD [ "software.amazonaws.example.product.handler.GetProductByIdHandler::handleRequest" ]