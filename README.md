# Add logging support to Simulation Execution Manager (SEM)
This page summarizes the work done as part of Google Summer of Code 2021 with with the [ns-3](https://gitlab.com/nsnam) Network Simulator.

### Project Overview
Simulation Execution Manager (SEM) is a Python library to perform multiple ns-3 script executions, manage the results and collect them in processing-friendly data structures. SEM tries to hide as many of the tedious details about running a simulation campaign as possible, providing a clean interface that helps the user get all the way from optimized compilation of ns-3 to plotting.

###### Initial status of SEM: 
At the time of starting this project, SEM did not provide an API or support for enabling logging in ns-3 simulations. Apart from this, SEM also did not have any support for visualization of ns-3 logs. Despite having no explicit support for logging, the users were still able to enable logging manually in their ns-3 scripts by using LogComponentEnable(). 

###### Project Goal: 
The project aims to do the following:
- Add support for ns-3’s built-in logging to SEM. 
- Add an interactive dashboard with different filters to efficiently visualize the ns-3 logs.



