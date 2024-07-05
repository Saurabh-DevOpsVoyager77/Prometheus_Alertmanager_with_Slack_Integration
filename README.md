**Setting Up Prometheus Alertmanager with Slack Integration**

![Your paragraph text](https://github.com/Saurabh-DevOpsVoyager77/Prometheus_Alertmanager_with_Slack_Integration/assets/147520862/155e8911-fa1c-4368-9b67-b9c7bb97d275)

**Introduction**

Integrating Prometheus Alertmanager with Slack can streamline the process of monitoring and alerting your Kubernetes clusters. Follow these steps to set up and configure Prometheus alerts to be sent to a Slack channel.

**Step 1: Clone the GitHub Repository**

Begin by cloning the repository that contains the necessary configurations.

`git clone https://github.com/Saurabh-DevOpsVoyager77/Prometheus_Alertmanager_with_Slack_Integration.git`

**Step 2: Configure Alerts**

Navigate to the Prometheus configuration directory and edit the config-map.yaml file.

`cd prometheus && vim config-map.yaml`

Add the following Prometheus rules to the configuration:

Send an alert if the status of any pod in the backend namespace fails for 1 minute.

Send an alert if the status is Pending, Unknown, Failed, or CrashLoopBackoff for more than 1 minute.

**Step 3: Apply Prometheus Manifests**

Create a namespace for monitoring and applying the Prometheus manifests.

`kubectl create ns monitoring`
`kubectl apply -f .`

**Step 4: Create Slack Webhook URL**

To integrate Alertmanager with a Slack channel, create a Slack webhook URL.

Create a channel on Slack.

Visit the Slack API page: Slack API

Sign in to your workspace and create a new app from scratch. Specify the app name and select the workspace.

In the settings menu on the left side, click on "Incoming Webhooks".

Activate Incoming Webhooks and copy the webhook URL.
![image](https://github.com/Saurabh-DevOpsVoyager77/Prometheus_Alertmanager_with_Slack_Integration/assets/147520862/81e3f3ee-a9ab-4437-96fc-0e9b3a7ac80c)
![image](https://github.com/Saurabh-DevOpsVoyager77/Prometheus_Alertmanager_with_Slack_Integration/assets/147520862/9db98e63-5556-492a-83f3-7ca7151c0712)



**Step 5: Add Slack Webhook URL to Alertmanager Configmap**

Navigate to the Alertmanager configuration directory and edit the AlertManagerConfigmap.yaml file.

`cd alert-manager`
`vim AlertManagerConfigmap.yaml`

Add the webhook URL and the Slack channel name to the configuration.

Apply the Alertmanager configuration:

`kubectl apply -f .`

**Step 6: Access the UI**

Verify that everything is running correctly. Port forward Prometheus and Alertmanager to access their UIs.

`kubectl port-forward service/prometheus-service 9090`
`kubectl port-forward service/alertmanager 9093`

Visit your browser's Prometheus and Alertmanager URLs to ensure the alerts are configured properly.

`http://localhost:9090/`

![image](https://github.com/Saurabh-DevOpsVoyager77/Prometheus_Alertmanager_with_Slack_Integration/assets/147520862/358ad9c6-0887-449c-84ce-c30d947cb2ce)


**Step 7: Test Alerts**

To test the alert configuration, add a test alert in the prometheus/config-map.yaml file.

- name: Test Alerts
  rules:
  - alert: TestAlert
    expr: vector(1)
    for: 1m
    labels:
      severity: slack
    annotations:
      summary: "This is a test alert"
      description: "This alert is for testing the Alertmanager integration."

Apply the updated Prometheus configuration and restart the Prometheus pod to load the new alert.

`kubectl apply -f prometheus/config-map.yaml`
`kubectl delete pod -l app=prometheus -n monitoring`

**Step 8: Verify in Slack**

Head over to the Slack channel you configured. You should see the test alert confirming the integration is working correctly.
![image](https://github.com/Saurabh-DevOpsVoyager77/Prometheus_Alertmanager_with_Slack_Integration/assets/147520862/391c146f-a9dc-48b8-9a40-fabbd9ac997c)

By following these steps, you can ensure that your Prometheus alerts are successfully sent to your Slack channel, helping you monitor the status of your Kubernetes clusters efficiently.




