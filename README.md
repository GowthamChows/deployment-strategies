# Blue-Green-Deployment-stratagey
Deploy Blue Application
    kubectl apply -f blue-deployment.yaml

Deploy Green Application
   kubectl apply -f green-deployment.yaml

Deploy Service
       kubectl apply -f svc-manifest.yaml

       And Then verify by 
           kubectl get all 

And then expose the port to the nodeport mode
   and the then acess the application.
   we can use the below "curl" command to test the traffic 
       $ for i in $(seq 1 10); do curl <app-url>; done | grep -o '<span id='\'podName\''>[^<]*' | sed 's/<[^>]*>//g'
       You Will see the traffic which is flowing into the blue-deployment

And after accessing the application And update the Green-deployment.yaml
      in green-deployment.yaml file change the image pod name  from image: gowtham35/echo-pod-name:v1.0.0 to gowtham/echo-pod-name:v2.0.0 
      so that we will deploy the New Version which is Updated version of Green-deployment.yaml.
      And now deploy the New Version Of green-deployment.yml
            kubectl apply -f green-deployment.yaml

Switch Traffic:
          Update the Service to Point Green deployment
            Here in the service File update the env:green
              so that The Trafiic is routed to the green deployment.
                 deploy the servive (svc-manifest.yaml)
                  kubectl apply -f svc-manifest.yaml
                   These changes will upgrade the green environment to the newer version (v2.0.0) compared to the blue environment (v1.0.0) and update the service to route traffic to the Green environment from the Blue environment.
Test & Monitor the New Version
    Or we can use "curl" command: to check the traffic 
      Now, the traffic is being routed to the new version (v2.0.0) from the Green Environment.

Roll Back 
      Sucessfully we have deployed a new version of application and the traffice is routed from blue deployment to green deployment. if you are encountered any issues with the new version which is freen deployment. we can rool back to the prevous version which is blue deployment. which is still avalible so we just need to update the selectors in service file (svc-manifest.yaml) "env: blue". so that the Traffic is again routed into the prevous version.
       deploy the service (svc-manifest.yaml) 
         kubectl apply -f svc-manifest.yaml

Blue-Green Deployment is a powerful strategy for deploying software updates with minimal downtime and risk. By setting up identical environments and switching traffic seamlessly, teams can release updates confidently while maintaining high availability. In Kubernetes, Blue-Green Deployment can be achieved effectively using namespaces and service routing.