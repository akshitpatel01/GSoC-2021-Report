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

For detailed design and implementation of each phase please refer to the [Design Document](https://docs.google.com/document/d/1GWQFEF1my4VmCnKayGZGYj6lwtYFQeE5qFI5emJlbOw/edit#).

## Project Contributions
This section covers the work done during the GSoC Period in terms of contributed code/documentation/tests.

### Phase 1: Add logging support to SEM
**Link to phase 1 PR**:

[https://github.com/signetlabdei/sem/pull/47](https://github.com/signetlabdei/sem/pull/47)

After discussing with the mentors on a detailed flow for phase 1, we modified the existing SEM run_missing_simulations API to allow the users to enable logging. SEM would also keep a track of logging simulatins in addition to non-logging simulations in its internal database. Towards the end of first phase, we added an example to demonstrte the new changes to SEM, tests (in pytest framework) to validate the changes and python docstrings for the documentation.

**Weekly progress for phase 1**: 



### Phase 2: Add APIs to read, parse, insert and filter logs
**Link to phase 2 PR**:

[https://github.com/signetlabdei/sem/pull/51](https://github.com/signetlabdei/sem/pull/51)


### Phase 3: Build an interactive dashboard 
**Link to phase 3 PR**:

[https://github.com/signetlabdei/sem/pull/54](https://github.com/signetlabdei/sem/pull/54)

## Final status of SEM

## Future Work

## Conclusion






