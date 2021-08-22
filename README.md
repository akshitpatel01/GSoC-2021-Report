# Add logging support to Simulation Execution Manager (SEM)
This page summarizes the work done as part of Google Summer of Code 2021 with the [ns-3](https://gitlab.com/nsnam) Network Simulator.

Student: [Akshit Patel](https://github.com/akshitpatel01)

Mentors: [Davide Magrin](https://github.com/DvdMgr), [Mattia Lecci](https://github.com/mattia-lecci)

## Project Overview
[Simulation Execution Manager (SEM)](https://github.com/signetlabdei/sem) is a Python library to perform multiple ns-3 script executions, manage the results and collect them in processing-friendly data structures. SEM tries to hide as many of the tedious details about running a simulation campaign as possible, providing a clean interface that helps the user get all the way from the optimized compilation of ns-3 to plotting.

### Initial status of SEM: 
At the time of starting this project, SEM did not provide an API or support for enabling logging in ns-3 simulations. Apart from this, SEM also did not have any support for the visualization of ns-3 logs. Despite having no explicit support for logging, the users were still able to enable logging manually in their ns-3 scripts by using/redefining [```LogComponentEnable()```](https://www.nsnam.org/doxygen/group__logging.html#gabe50035652d407c40bdaef78214c4955). 

### Project Goal: 
The project aims to do the following:
- Add support for ns-3’s built-in logging to SEM. We aim to provide an API that enables users to select the log components and their respective log severity classes to enable while running ns-3 simulations using SEM. 
- Add an interactive dashboard with different filters to efficiently visualize the ns-3 logs. This dashboard API is independent of the SEM logging module and hence allows the users to pass in any ns-3 generated log file (i.e. the users can pass a log file generated directly from SEM or manually by ns-3). 

The Project was divided into four phases:
- **Phase 1**: Modify the existing run_missing_simulations API to add logging support in SEM along with tests and documentation of the functions added/modified.
- **Phase 2**: Add APIs to read, parse and filter logs from an ns-3 generated log file along with an API to insert the parsed logs to a TinyDB database instance.
- **Phase 3**: Build frontend as well as backend components for the interactive dashboard.
- **Phase 4**: Add support for logging and accessing the dashboard from SEM-CLI. 

## Project Contributions
This section covers the work done during the GSoC Period in terms of contributed code/documentation/tests. For detailed design and implementation of each phase please refer to the [Design Document](https://docs.google.com/document/d/1GWQFEF1my4VmCnKayGZGYj6lwtYFQeE5qFI5emJlbOw/edit?usp=sharing).

### Phase 1: Add logging support to SEM
**Link to phase 1 PR**:

[https://github.com/signetlabdei/sem/pull/47](https://github.com/signetlabdei/sem/pull/47)

After discussing with the mentors on a detailed flow for phase 1, we modified the existing SEM [run_missing_simulations](https://simulationexecutionmanager.readthedocs.io/en/develop/api.html#sem.CampaignManager.run_missing_simulations) API to allow the users to enable logging. SEM would also keep a track of logging simulations in addition to non-logging simulations in its internal database. Towards the end of the first phase, we added an [example](https://github.com/akshitpatel01/sem/blob/gsoc2021/examples/logging_example.py) to demonstrate the new changes to SEM, tests (in pytest framework) to validate the changes and python docstrings for the documentation.

**Weekly progress for phase 1**

- **Week 1**: Modified existing (relevant) SEM APIs to take enabled log components as an argument. Added functionality to parse user input either in dictionary format or NS_LOG-formatted string. Added a check to verify if the log components passed are valid.

- **Week 2**: Created NS_LOG environment variable based on user input and pass the environment to SimulationRunner to execute the simulations within that environment. Added a check to verify SEM is build in debug mode. Add a sample ns-3 example in sem/ns-3 fork to test the logging module.  

- **Week 3**: Modify DatababseManager APIs to store logging simulations in addition to non-logging simulations in the internal SEM database. Added python docstrings as documentation for the added/modified functions. Added an example to demonstrate the logging workflow and tests to validate the new changes.


### Phase 2: Add APIs to read, parse, insert and filter logs
**Link to phase 2 PR**:

[https://github.com/signetlabdei/sem/pull/51](https://github.com/signetlabdei/sem/pull/51)

This phase focuses on creating APIs dedicated to parsing the log files passed in by the user. These APIs allow the user to filter log files based on 1) Time 2) Context 3) Function 4) Component. Also, the APIs created in this phase will be used by the backend of the dashboard created in the next phase. Towards the end of the second phase, we added an [example](https://github.com/akshitpatel01/sem/blob/gsoc2021/examples/logging_example2.py) to demonstrate the new changes to SEM, tests (in pytest framework) to validate the changes and python docstrings for the documentation.

**Weekly progress for phase 2**

- **Week 4**: Added regex for validating and parsing the logs from the provided log file. Added API to insert parsed logs into a database.
 
- **Week 5**: Added an API to query the database based on the filters passed. Added an example to demonstrate the new APIs and test to validate the new changes.

- **Week 6**: Performed [profiling experiments](https://github.com/akshitpatel01/sem/tree/profiling/profiling) on these new APIs as these functions are designed to be used by the backend. Found that these APIs take an infeasible amount of time to run and tuned the APIs to reduce the time taken to execute. 


### Phase 3: Build an interactive dashboard 
**Link to phase 3 PR**:

[https://github.com/signetlabdei/sem/pull/54](https://github.com/signetlabdei/sem/pull/54)

This phase focuses on creating an interactive dashboard for visualizing ns-3 log files. The front end of the dashboard is built using HTML, CSS, and JQuery. The dashboard has two main components 1) Graph 2) Table. Two Jquery based libraries have been used to create the dashboard: 1) [chart.js](https://www.chartjs.org/) (for graph) 2) [Datatables](https://datatables.net/) (for table). For the backend, [flask](https://flask.palletsprojects.com/en/2.0.x/) has been used. The backend also makes use of the APIs written in phase 2. For a complete overview of the features of the dashboard, refer [here](https://github.com/akshitpatel01/sem/blob/gsoc2021/sem/dashboard/README.md).  At the time of writing this report, the phase 3 PR is yet to be merged (Last commit: [here](https://github.com/signetlabdei/sem/pull/54/commits/9f1e53900d2ad1261bf16a5662e7f77f6558f0bd)). 

**Weekly progress for phase 3**

- **Week 7**: Created a basic flask app for the backend and linked it with a sample table created using datatables in the frontend. 

- **Week 8**: Worked on APIs for the frontend to communicate with the backend. Added dropdown filters in the frontend for better log visualization. 

- **Week 9**: Added the graph for visualizing the logs against time. Added APIs to accommodate the new frontend components. 

- **Week 10**: Worked on improving the performance and features of the dashboard. Created a markdown file listing the features of the current dashboard. Added an [example](https://github.com/akshitpatel01/sem/blob/gsoc2021/examples/logging_example2.py) to demonstrate the use of dashboard. 


## Final status of SEM 
The final GSoC code is available [here](https://github.com/akshitpatel01/sem/tree/gsoc2021-final). Below are the user-visible features added to SEM during GSoC:
- SEM provides explicit API support to enable logging (of any number of log components with any log level/class enabled) in simulations.
- SEM provides APIs to read, parse and filter logs from ns-3 generated log files.
- SEM allows the users to visualize the ns-3 generated log files using an interactive dashboard with different filters. Users can even use this dashboard for visualizing log files directly generated by ns-3 in addition to the log files generated by SEM simulations. 


## Future Work
Unfortunately, not all the goals mentioned in the project are accomplished. Adding support for enabling logging or accessing the dashboard is yet to be added to SEM-CLI. Other than this, all the tasks have been almost complete. 

After discussion with the mentors, possible future work for SEM is as follows:

- **Add support for logging and accessing the dashboard from SEM-CLI**: As this task could not be completed at the time of writing this report, it will be done after GSoC ends.

- **Add support for time filtering of logs in ns-3**: Ns-3 does not support time filtering of logs as of now. This feature will be useful for the users as the log files generated by ns-3 can be huge and this might directly affect the responsiveness of the SEM dashboard. For this task, Mattia Lecci has already opened up an [MR](https://gitlab.com/nsnam/ns-3-dev/-/merge_requests/636) (yet to be merged).  

- **Add support for custom context in SEM**: Provide support for custom context created by users (using NS_LOG_APPEND_CONTEXT). Currently, ns-3 allows users to redefine/use NS_LOG_APPEND_CONTEXT and add a custom string to the logs. As there are no clear start and end marks for the extended context, this task is yet to be done.

I will gladly help if any issues come up with the new code added.

## Conclusion
I am very thankful to my family, mentors, and the ns-3 community who helped and supported me throughout GSoC. I am also thankful to Dr. Mohit P. Tahiliani for encouraging me to apply for the program. Though my project comes to an end, I will keep contributing to open source as well as SEM. 
