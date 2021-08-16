# Add logging support to Simulation Execution Manager (SEM)
This page summarizes the work done as part of Google Summer of Code 2021 with with the [ns-3](https://gitlab.com/nsnam) Network Simulator.

## Project Overview
Simulation Execution Manager (SEM) is a Python library to perform multiple ns-3 script executions, manage the results and collect them in processing-friendly data structures. SEM tries to hide as many of the tedious details about running a simulation campaign as possible, providing a clean interface that helps the user get all the way from optimized compilation of ns-3 to plotting.

### Initial status of SEM: 
At the time of starting this project, SEM did not provide an API or support for enabling logging in ns-3 simulations. Apart from this, SEM also did not have any support for visualization of ns-3 logs. Despite having no explicit support for logging, the users were still able to enable logging manually in their ns-3 scripts by using LogComponentEnable(). 

### Project Goal: 
The project aims to do the following:
- Add support for ns-3’s built-in logging to SEM. We aim to provide an API which enables users to select the log components and their respective log severity classes to enable while running ns-3 simulations using SEM. 
- Add an interactive dashboard with different filters to efficiently visualize the ns-3 logs. This dashboard API is independent to the SEM logging module and hence allows the users to pass in any ns-3 generated log file (i.e. the users can pass a log file generated directly from SEM or manually by ns-3). 

The Project was divided into four phases:
- **Phase 1**: Modify the existing run_missing_simulations API to add logging support in SEM along with tests and documentation of the functions added/modified.
- **Phase 2**: Add APIs to read, parse and filter logs from an ns-3 generated log file along with an API to insert the parsed logs to a TinyDB database instance.
- **Phase 3**: Build frontend as well as backend components for interactive dashboard.
- **Phase 4**: Add support for logging and accessing the dashboard from SEM-CLI. 

## Project Contributions
This section covers the work done during the GSoC Period in terms of contributed code/documentation/tests. For detailed design and implementation of each phase please refer to the [Design Document](https://docs.google.com/document/d/1GWQFEF1my4VmCnKayGZGYj6lwtYFQeE5qFI5emJlbOw/edit#).

### Phase 1: Add logging support to SEM
**Link to phase 1 PR**:

[https://github.com/signetlabdei/sem/pull/47](https://github.com/signetlabdei/sem/pull/47)

After discussing with the mentors on a detailed flow for phase 1, we modified the existing SEM run_missing_simulations API to allow the users to enable logging. SEM would also keep a track of logging simulatins in addition to non-logging simulations in its internal database. Towards the end of first phase, we added an example to demonstrte the new changes to SEM, tests (in pytest framework) to validate the changes and python docstrings for the documentation.

**Weekly progress for phase 1**

- **Week 1**: Modified exisiting (relevant) SEM APIs to take enabled log components as an argument. Added functionality to parse user input either in dictionary format or NS_LOG-formatted string. Added a check to verity if the log components passed are valid.

- **Week 2**: Created NS_LOG environment variable based on user input and pass the environment to SimulationRunner to execute the simulations within that environment. Added a check to verify SEM is build in debug mode. Add a sample ns-3 example in sem/ns-3 fork to test the logging module.  

- **Week 3**: Modify DatababseManager APIs to store logging simulations in addition to non-logging simulations in the internal SEM database. Added python docstrings as documentation for the added/modified functions. Added an example to demonstrate the logging workflow and tests to validate the new changes.


### Phase 2: Add APIs to read, parse, insert and filter logs
**Link to phase 2 PR**:

[https://github.com/signetlabdei/sem/pull/51](https://github.com/signetlabdei/sem/pull/51)

This phase focusses on creating APIs dedicated towards parsing the log files passed in by the user. These APIs allow the user to filter log files based on 1) Time 2) Context 3) Function 4) Component. Also the APIs created in this phase will be used by backend of the dashboard created in next phase. Towards the end of second phase, we added an example to demonstrte the new changes to SEM, tests (in pytest framework) to validate the changes and python docstrings for the documentation.

**Weekly progress for phase 2**

- **Week 4**: Added regex for validating and parsing the logs from the provided log file. Added API to insert parsed logs into a database.
 
- **Week 5**: Added an API to query the database based on the filters passed. Added an example to demonstrate the new APIs and test to validate the new changes.

- **Week 6**: Performed profiling experiments on these new APIs as these functions are designed to be used by backend. Found that these APIs take an infeasible amount of time to run and modified the APIs to reduce the time taken to execute. 


### Phase 3: Build an interactive dashboard 
**Link to phase 3 PR**:

[https://github.com/signetlabdei/sem/pull/54](https://github.com/signetlabdei/sem/pull/54)

This phase focusses on creating a interactive dashboard for visualizing ns-3 log files. The frontend of the dashboard is built using html, css and JQuery. The dasboard has two main components 1) Graph 2) Table. Two Jquery based libraries have been used to create the dasboard: 1) chart.js (for graph) 2) Datatables (for table). For the backend, flask has been used. The backend also makes use of the APIs written in phase 2. For a complete overview of the features of the dashboard, refer [here]().  

**Weekly progress for phase 3**

- **Week 7**: Created a basic flask app for backend and linked it with a sample table created using datatables in the frontend. 

- **Week 8**: Worked on APIs for frontend to communicate with the backend. Added dropdown filters in the frontend for better log visualization. 

- **Week 9**: Added the graph for visualizing the logs against time. Added APIs to accomodate the new frontend components. 

- **Week 10**: Worked on improving the performance of the dashboard. Created a markdown file listing the features of the current dashboard. 

## Final status of SEM

## Future Work

## Conclusion






