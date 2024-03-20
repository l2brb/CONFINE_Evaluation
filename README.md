# CONFINE Evaluation
 
## Evaluation
The following section contains the experimental toolbench used to evaluate the effectiveness of CONFINE, presented in the paper Section 6. Evaluation files can be found in [/evaluation/](https://github.com/Process-in-Chains/CONFINE/tree/main/evaluation). We conduct convergence analysis to demonstrate the correctness of the collaborative data exchange process. Moreover, we gauge the memory usage with synthetic and real-life event logs, to observe the trend during the enactment of our protocol and assess scalability. 

##### Requirements
To run our Python scripts, the following libraries are required: `os`, `pandas`, `numpy`, `matplotlib`, `scipy`, `sklearn`, `datetime`.

### Tests
For each test we used the event log of the motivating scenario and additionally, the [BPIC](https://www.tf-pm.org/competitions-awards/bpi-challenge) logs, specifically [Sepsis](https://data.4tu.nl/datasets/33632f3c-5c48-40cf-8d8f-2db57f5a6ce7) and [BPIC 2013](https://data.4tu.nl/datasets/1987a2a6-9f5b-4b14-8d26-ab7056b17929) event logs. We further processed these logs to simulate an inter-organizational scenario. We made specific modifications on the scalability tests event logs to allow the observation of different configurations of number of events per case, number of cases and number of provisioning organizations. To run the test, the specific commands to be execute in the terminal are the following:

###### Miner
`ego-go build -buildvcs=false && ego sign enclave.json && OE_SIMULATION=1 ego run ./app -segsize ${SEGSIZE_VALUE} -port ${PORT_NUMBER} -test true`

###### Provisioner
`go build -o logprovision log_provision/log_provision.go && ./logprovision -port ${PORT_NUMBER} -log ${LOG_PATH}`

#####  Output Convergence

To experimentally validate the correctness of our approach in the transmission and computation phases, we run a [convergence](https://github.com/Process-in-Chains/CONFINE/tree/main/evaluation/convergence) test. To this end, we created a synthetic event log (available in [/event_log/](https://github.com/Process-in-Chains/CONFINE/tree/main/evaluation/convergence/event_log)) consisting of 1000 cases of 14 events on average by simulating the inter-organizational process of our motivating scenario and we partitioned it in three sub-logs (Respectively Hospital, Specialized clinic and Pharmaceutical company event logs). We run the stand-alone HeuristicsMiner on the former, and processed the latter through our CONFINE toolchain. The convergence results are available in [/output/](https://github.com/Process-in-Chains/CONFINE/tree/main/evaluation/convergence/output) in the form of a workflow net.


##### Memory Usage
To evaluate the runtime memory utilization of our CONFINE implementation, we run a [memory usage](https://github.com/Process-in-Chains/CONFINE/tree/main/evaluation/memoryusage) test, split into four different configurations:
- In the first test, we excluded the computation phase by leaving the HeuristicsMiner inactive, so as to isolate execution from mining-specific operations. In this case we set the value of the segment size `${SEGSIZE_VALUE}` to 2000 KiloBytes and we used the same synthetic [event_log](https://github.com/Process-in-Chains/CONFINE/tree/main/evaluation/memoryusage/event_log/motivating_scenario) of our motivating scenario. 
- In the second test, on the other hand, we included the computation phase using the same synthetic event log and setting the segment size value as in the first test. The results of the first two tests are available in [/output_test_motivating_scenario/](https://github.com/Process-in-Chains/CONFINE/blob/main/evaluation/memoryusage/output_test_motivating_scenario/test_mem.csv). 
- In the third test, we also gauged the runtime memory usage of two public real-world event logs too. Since those are intra-organizational event logs, we split the contents to mimic an inter-organizational context. In particular, we separated the [Sepsis](https://github.com/Process-in-Chains/CONFINE/tree/main/evaluation/memoryusage/event_log/sepsis) event log based on the distinction between normal-care and intensive-care paths, as if they were conducted by two distinct organizations. Similarly, we processed the [BPIC 2013](https://github.com/Process-in-Chains/CONFINE/tree/main/evaluation/memoryusage/event_log/bpic13) event log to sort it out into the three departments of the Volvo IT incident management system.
- In the fourth test, we conducted an additional test to examine the trend of memory usage as the segment size varies with all the aforementioned event logs. The results of the test are available in [/output_test_segment_size/](https://github.com/Process-in-Chains/CONFINE/tree/main/evaluation/memoryusage/output_test_segment_size).


##### Scalability
We examine the [scalability](https://github.com/Process-in-Chains/CONFINE/tree/main/evaluation/scalability) of the Secure Miner, focusing on its capacity to efficiently manage an increasing workload in the presence of limited memory resources. We implemented three distinct test configurations gauging runtime memory usage as variations of our motivating scenario log.

- To conduct the test on the maximum number of events, we modified the motivating scenario [event log](https://github.com/Process-in-Chains/CONFINE/tree/main/evaluation/scalability/event_log/log_test_maxevents) by adding a loop back from the final to the initial activity of the process model, progressively increasing the number of iterations. The results of the test are available in [/output_test_max_events/](https://github.com/Process-in-Chains/CONFINE/tree/main/evaluation/scalability/output_test_max_events).
- Concerning the test on the number of cases, we simulated additional process instances, building new  [event logs](https://github.com/Process-in-Chains/CONFINE/tree/main/evaluation/scalability/event_log/log_test_cases).  The results of the test are available in [/output_test_cases/](https://github.com/Process-in-Chains/CONFINE/tree/main/evaluation/scalability/output_test_cases).
- Finally, for the assessment of the number of organizations, the test necessitated the distribution of the process model activities’ into a variable number of pools, each representing a different organization. Event logs are available in [/log_test_organizations/](https://github.com/Process-in-Chains/CONFINE/tree/main/evaluation/scalability/event_log/log_test_organizations), results of the test are available in [/output_test_organizations/](https://github.com/Process-in-Chains/CONFINE/tree/main/evaluation/scalability/output_test_organizations).
