FROM amazoncorretto:21-alpine 
  
# Copy function code and runtime dependencies from Maven layout
COPY target/classes ${LAMBDA_TASK_ROOT}
COPY target/dependency/* ${LAMBDA_TASK_ROOT}/lib/
    
# Set runtime interface client as default command for the container runtime
ENTRYPOINT [ "/usr/bin/java", "-cp", "${LAMBDA_TASK_ROOT}/*", "com.amazonaws.services.lambda.runtime.api.client.AWSLambda" ]    

# Set the CMD to your handler (could also be done as a parameter override outside of the Dockerfile)
# CMD [ "software.amazonaws.example.product.handler.GetProductByIdHandler::handleRequest" ]  