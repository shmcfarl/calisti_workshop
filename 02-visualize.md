

# Visualize Your Mesh

## Topology 

The TOPOLOGY page of the Calisti web interface displays the topology of services and workloads inside the mesh, and annotates it with real-time information about latency, throughput, or HTTP request failures. You can also display historical data by adjusting the timeline.

The topology view is almost entirely based on metrics received from Prometheus and enhanced with information from Kubernetes.

The topology page serves as a starting point of diagnosing problems within the mesh. Calisti is integrated with Grafana and Jaeger for easy access to in-depth monitoring and distributed traces of various services.

If not already selected, select the "smm-demo" namespace and display its topolgy.

![topology 1](images/m1_3.png)

The nodes in the graph are services or workloads, while the arrows represent network connections between different services. This is based on Istio metrics retrieved from Prometheus. You can click and zoom into the services and note how the traffic protocols along with the requests per second (RPS) are also shown in the topology view.

We can easily observe the various microservices in the demo application:

•	The frontpage microservice calls bookings, catalog and postgresql microservices to populate the page

•	The bookings microservice calls the analytics and payments microservices 

•	The payments microservice calls the notifications microservice

•	The catalog microservice calls the movie microservices.

•	There are 3 versions of the catalog microservice with version2 using mysql and version3 using a different database

### View Database Metrics

Calisti is also able to show the details for services such as MySQL and Postgresql – these metrics are not available in Istio and is a value-add provided by Calisti. 

Select the postgresql service and in the pop-up window, scroll down to note how it shows the details such as SQL transactions per second, etc.  

![calisti dashboard 6](images/1_8.png)


Select the "postgresql (kind-demo1)" service in the graph and display its details. By drilling down and selecting the pod it is also possible to display its logs directly in the dashboard (click on the ![log](images/log_icon.png) icon) NOTE: The log window will be at the lower-right corner of the window. You may have to drag the top of the log window up to see it more easily.

![topology 1](images/pod_logs.png)

