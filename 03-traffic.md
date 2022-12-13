
# Traffic Management

## Traffic Tap

The traffic tap feature of Calisti enables you to monitor live access logs of the Istio sidecar proxies. Each sidecar proxy outputs access information for the individual HTTP requests or HTTP/gRPC streams.

The access logs contain information about the reporter proxy, source and destination workloads, request, response, as well as the timings.

### Traffic Tap Using the UI

Select the menu at the top-left of the screen and select "TRAFFIC TAP" then select "smm-demo" in the "REPORTING SOURCE" list. Select "START STREAMING".

After a few seconds, select "PAUSE STREAMING".

Select any row of a trace to see more information about that trace.

The functionality is also available in the CLI, including setting the different filters (check the Calisti documentation).

![ttapui 1](images/ttapui_1.png)

## Fault Injection

Fault injection is a system testing method which involves the deliberate introduction of network faults and errors into a system. It can be used to identify design or configuration weaknesses, and to ensure that the system can handle faults and recover from error conditions.

With Calisti, you can inject failures at the application layer to test the resiliency of the services. You can configure faults to be injected into requests that match specific conditions to simulate service failures and higher latency between services. There are two types of failures:

**Delay** - Adds a time delay before forwarding the requests, emulating various failures such as network issues, an overloaded upstream service, and so on.

**Abort** - Aborts the HTTP request attempts and returns error codes to a downstream service, giving the impression that the upstream service is faulty.

Calisti uses Istio’s (Envoy) fault injection feature under the hood.

Select the "bookings" service in the "TOPOLOGY" view, select "TRAFFIC MANAGEMENT" and "CREATE NEW".

![fault 1](images/fault_1.png)

Set the following values:
- PORT NUMBER: 8080
- FAULT INJECTION:
  - DELAY PERCENTAGE: 50
  - FIXED DELAY: 3s
  - ABORT PERCENTAGE: 40
  - ABORT HTTP STATUS CODE: 503

![fault 2](images/fault_2.png)
![fault 3](images/fault_3.png)

Select "Apply". Under the "OVERVIEW" tab, you can see the ERROR RATE increasing. You can see the status of the "Health" bar moving towards the "unhealthy" side.


![fault 5](images/fault_5.png)

To remove the Fault Injection, select the "TRAFFIC MANAGEMENT" tab and click on the trash can icon to the far right of the fault policy and then confirm the route deletion by selecting "Delete".

## Traffic Steering/Splitting (Canaries, Blue/Green)

Application service meshes support the use of Traffic Steering, AKA: Traffic Splitting. This functionality provides a way to have multiple versions of a service and then 'steer' traffic to each version of that service by a percentage of traffic. This is an awesome way of testing out brand new code or new capablities using live traffic.

In the "TOPOLOGY" view, notice that under the "movies" service there are v1, v2 and v3 workloads. In this excercise you will steer traffic to only the v3 service.

![traffic m1](images/mtraffic_1.png)

Select the "movies" service, then "TRAFFIC MANAGEMENT". You can see a pre-defined traffic management rule. You will notice that the defined traffic splitting ratio is 33% for each version of the movies service.

![traffic m2](images/mtraffic_2.png)

Select the pencil icon on the far right of the existing traffic management rule. 

Set the following values:
- Click the red X on the first row to delete the v1 subset
- Click the red X on the second row to delete the v2 subset
- SUBSET v3 - change the WEIGHT: 100

Select "Apply"

![traffic m3](images/mtraffic_3.png)


Select the "v3" workload and under the "OVERVIEW" tab, scroll down and you will see an increase in the "INCOMING REQUEST BY DESTINATION" metric the "movies-v3.smm-demo".

### Distributed Tracing

Calisti also provides distributed tracing - the process of tracking individual requests throughout their whole call stack in the system.

With distributed tracing in place it is possible to visualize full call stacks, to see which service called which service, how long each call took and how much the network latencies were between them. It is possible to tell where a request failed or which service took too much time to respond.
To collect and visualize this information, Istio comes with tools like Jaeger which is installed automatically by default when installing Calisti.

The demo application uses golang services which are configured to propagate the necessary tracing headers.

Once load is sent to the application, traces can be perceived right away.
Jaeger is exposed through an ingress gateway and the links are present on the UI (both on the graph and list view). 

Select the menu item at the top-left of the screen and select "TOPOLOGY". Select the "bookings" service then select the "Traces" item that is on the lower-right hand side of the window (in the "OVERVIEW" tab).


![ttapui 2](images/ttapui_2.png)


In the Jaeger UI you can see the whole call stack in the microservices architecture. Click on one of the trace titles (e.g., "bombardier.smm-demo: frontpage.smm-demo.svc.cluster.local:8080"). You can see when the root request was started and how much time each request took as it hits each service. 


![ttapui 3](images/ttapui_3.png)


## Conclusion

This concludes the Calsti hands-on lab. 

Navigate to https://calisti.app. Click on the “Sign up, It’s Free” button and proceed to register and download the Calisti binaries.

![calisti register](images/1_1.png)

