## Getting Started Running Chaos Scenarios

#### Adding New Scenarios
Adding a new scenario is as simple as adding a new config file under [scenarios directory](https://github.com/chaos-kubox/krkn/tree/main/scenarios) and defining it in the main kraken [config](https://github.com/chaos-kubox/krkn/blob/main/config/config.yaml#L8).
You can either copy an existing yaml file and make it your own, or fill in one of the templates below to suit your needs.

### Templates
#### Pod Scenario Yaml Template
For example, for adding a pod level scenario for a new application, refer to the sample scenario below to know what fields are necessary and what to add in each location:
```
config:
  runStrategy:
    runs: <number of times to execute the scenario>
    #This will choose a random number to wait between min and max
    maxSecondsBetweenRuns: 30
    minSecondsBetweenRuns: 1
scenarios:
  - name: "delete pods example"
    steps:
    - podAction:
        matches:
          - labels:
              namespace: "<namespace>"
              selector: "<pod label>"  # This can be left blank.
        filters:
          - randomSample:
              size: <number of pods to kill>
        actions:
          - kill:
              probability: 1
              force: true
    - podAction:
        matches:
          - labels:
              namespace: "<namespace>"
              selector: "<pod label>"  # This can be left blank.
        retries:
          retriesTimeout:
            # Amount of time to wait with retrying, before failing if pod count does not match expected
            # timeout: 180.

        actions:
          - checkPodCount:
              count: <expected number of pods that match namespace and label"
```

More information on specific items that you can add to the pod killing scenarios can be found in the [powerfulseal policies](https://powerfulseal.github.io/powerfulseal/policies) documentation


#### Node Scenario Yaml Template

```
node_scenarios:
  - actions:  # Node chaos scenarios to be injected.
    - <chaos scenario>
    - <chaos scenario>
    node_name: <node name>  # Can be left blank.
    label_selector: <node label>
    instance_kill_count: <number of nodes on which to perform action>
    timeout: <duration to wait for completion>
    cloud_type: <cloud provider>
```


#### Time Chaos Scenario Template
```
time_scenarios:
  - action: 'skew_time' or 'skew_date'
    object_type: 'pod' or 'node'
    label_selector: <label of pod or node>
```


### Common Scenario Edits
If you just want to make small changes to pre-existing scenarios, feel free to edit the scenario file itself.

#### Example of Quick Pod Scenario Edit:
If you want to kill 2 pods instead of 1 in any of the pre-existing scenarios, you can either edit the number located at filters -> randomSample -> size or the runs under the config -> runStrategy section.

#### Example of Quick Nodes Scenario Edit:
If your cluster is build on GCP instead of AWS, just change the cloud type in the node_scenarios_example.yml file.
