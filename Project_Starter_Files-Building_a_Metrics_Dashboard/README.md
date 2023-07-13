**Note:** For the screenshots, you can store all of your answer images in the `answer-img` directory.

## Verify the monitoring installation

*TODO:* run `kubectl` command to show the running pods and services for all components. Take a screenshot of the output and include it here to verify the installation
![Screenshot of running pods and services for all components](/answer-img/pods_services_screenshot.png)


## Setup the Jaeger and Prometheus source
*TODO:* Expose Grafana to the internet and then setup Prometheus as a data source. Provide a screenshot of the home page after logging into Grafana.
![Screenshot of Grafana home page](/answer-img/grafana_screenshot.png)

## Create a Basic Dashboard
*TODO:* Create a dashboard in Grafana that shows Prometheus as a source. Take a screenshot and include it here.
![Screenshot of Grafana data source Prometheus](/answer-img/grafana_datasources.png)

## Describe SLO/SLI
*TODO:* Describe, in your own words, what the SLIs are, based on an SLO of *monthly uptime* and *request response time*.
We have the following SLOs:
1. The application will have an uptime of 99.9% at each month.
2. The average time taken to return a request will be less than 200 ms, during the month.

For the SLOs above we can describe SLIs as below:
1. We achieved 99.5% uptime during the month of May.
2. The average time taken to return a request during the month of May was 190 ms.

## Creating SLI metrics.
*TODO:* It is important to know why we want to measure certain metrics for our customer. Describe in detail 5 metrics to measure these SLIs.
1. Requests per second: Requests per second, or throughput, measures how many requests an application, website or software program receives each second. Typically, more requests per second can result in slower response times.
2. Error Rate: The percentage of errors that occur during a specific time period. This can help identify issues with the web application, such as server downtime, broken links or failed requests.
3. Average response time: Average response time (ART) is a measurement of the amount of time a server or application takes to respond to all of its data inputs and requests. A lower average response time typically means better performance, as the server or application takes less time to respond to new requests.
4. Peak response time: Measures the longest response time for a total number of requests that travel across the server.
5. CPU usage: CPU usage affects the responsiveness of an application. High spikes in CPU usage might indicate several problems.

## Create a Dashboard to measure our SLIs
*TODO:* Create a dashboard to measure the uptime of the frontend and backend services We will also want to measure to measure 40x and 50x errors. Create a dashboard that show these values over a 24 hour period and take a screenshot.
![Screenshot of dashboard to measure our SLIs](/answer-img/sli_dashboard.png)

## Tracing our Flask App
*TODO:*  We will create a Jaeger span to measure the processes on the backend. Once you fill in the span, provide a screenshot of it here. Also provide a (screenshot) sample Python file containing a trace and span code used to perform Jaeger traces on the backend service.
![Screenshot of Jaeger span to measure the processes on the backend.](/answer-img/bakend_service_span.png)

## Jaeger in Dashboards
*TODO:* Now that the trace is running, let's add the metric to our current Grafana dashboard. Once this is completed, provide a screenshot of it here.
![Screenshot of Jaeger metric on Grafana dashboard.](/answer-img/grafana_flask_metrics_backend_dashboard.png)

## Report Error
*TODO:* Using the template below, write a trouble ticket for the developers, to explain the errors that you are seeing (400, 500, latency) and to let them know the file that is causing the issue also include a screenshot of the tracer span to demonstrate how we can user a tracer to locate errors easily.
[!Screenshot of tracer span](/answer-img/backend_service_slowness_span.png)

TROUBLE TICKET

Name: Mustafa Behadir

Date: 12/07/2023

Subject: Latency in the slowness endpoint

Affected Area: API requests

Severity: High

Description: Backend service endpoint slowness has significant latency in process. You can see the trace of endpoint in the image backend_service_slowness_span.png.


## Creating SLIs and SLOs
*TODO:* We want to create an SLO guaranteeing that our application has a 99.95% uptime per month. Name four SLIs that you would use to measure the success of this SLO.
1. 2XX, 3XX, 4XX (except 429 rate limiting) / Total Requests should be greater or equal to 99.95% per month.
2. %90 of requests should be processed less than 150 ms.
3. Peak CPU usage per pod should not be more than %80 and average CPU usage per pod should be less than %50.
4. Peak memory usage per pod should not be more than %80 and average memory usage per pod should be less than %50.


## Building KPIs for our plan
*TODO*: Now that we have our SLIs and SLOs, create a list of 2-3 KPIs to accurately measure these metrics as well as a description of why those KPIs were chosen. We will make a dashboard for this, but first write them down here.
1. Maximum error rate should be less than %5 last 24 hours:
     - Error Rate can be used for this measurement. It measures total error rate with 4xx and 5xx. 4xx errors are mostly caused by frontend application and 5xx errors are mostly caused by backend application.
2. Request average duration should be less than 150 ms:
     - Requests Average Duration can be used for this measurement. Users can not realize the latencies less than 150 ms. If latency is more than usual it can give a sign of a bad design, coding or lack of resources.

## Final Dashboard
*TODO*: Create a Dashboard containing graphs that capture all the metrics of your KPIs and adequately representing your SLIs and SLOs. Include a screenshot of the dashboard here, and write a text description of what graphs are represented in the dashboard.
![Screenshot of final dashboard](/answer-img/final_dashboard.png)

Dashboard can be found in answer-img folder with the name final dashboard.
- Total Requests: Total number of all api requests to backend and frontend application endpoints.
- Requests Per Status Code: Total number of all api requests by status code return from backend/app.py and frontend/app.py endpoints.
- Pod UP Time: Backend and frontend containers pod reachability with time.
- CPU Usage Per Pod: Backend and frontend containers pod CPU usage information.
- Requests Per Second: Number of requests handled by backendand frontend application endpoints.
- Requests Average Duration: Request average process time by backend and frontend applications.
- Error Rate: Total error rate with 4xx and 5xx.
- Peak Response Time: Request peak process time by backend and frontend applications.
- Flask Metrics: Jaeger traces.


## NOTES
*HELM Installation*:
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash

*FORWARD PORTS*: 
kubectl port-forward service/prometheus-grafana --address 0.0.0.0 3000:80 -n monitoring
kubectl port-forward service/prometheus-kube-prometheus-prometheus --address 0.0.0.0 9090:9090 -n monitoring
kubectl port-forward service/my-traces-query --address 0.0.0.0 16686:16686 -n observability
kubectl port-forward replicaset/backend-f87b574b9 --address 0.0.0.0 8081:8081

*CREATE HTTP REQUESTS BACKEND*:
for i in {1..100}; do curl 127.0.0.1:8081; done