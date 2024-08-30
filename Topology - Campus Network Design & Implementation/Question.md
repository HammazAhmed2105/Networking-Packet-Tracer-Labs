# Albio University Network Topology

## Scenario Overview
Albio University is a large institution with two campuses located 20 miles apart. The university has four faculties: Health and Science, Business, Engineering/Computing, and Art/Design. Staff members are equipped with PCs, and students have access to PCs in labs.

## Requirements

### 1. Main Campus
- **Building A**:
  - **Departments**: Management, HR, and Finance.
  - **Faculty**: Business.
  - **Networking**: Administrative staff PCs are distributed across offices and share networking equipment. VLANs are expected to segregate different departments.
  
- **Building B**:
  - **Faculties**: Engineering and Computing, Art and Design.
  
- **Building C**:
  - **Purpose**: Student labs and the IT Department.
  - **Networking**: The IT department hosts the university's web server and other internal servers.
  - **External Servers**: There is an email server hosted externally on the cloud.

### 2. Smaller Campus
- **Faculty**: Health and Sciences.
- **Setup**: Staff and student labs are located on different floors.

## Configuration Guidelines
- **Core Device Configuration**: Configure core devices and a few end devices to ensure end-to-end connectivity and access to internal and external servers.
- **Departmental Networks**: Each department should be placed on its own separate network.
- **Routing**: Use RIPv2 for internal network routing and static routing for the external server.
- **Dynamic IP Assignment**: Devices in Building A will obtain dynamic IP addresses from a router-based DHCP server.

## Tasks

### Task 1: Plan, Design, and Prototype the Network Topology
**Objective**: Develop a comprehensive network topology that includes the following components:
- **Main Campus**:
  - VLAN configurations for departments in Building A.
  - Segmentation of faculties in Buildings B and C.
  - Server placement and network segmentation in Building C.

- **Smaller Campus**:
  - Network configuration for the Faculty of Health and Sciences, ensuring separate networks for staff and student labs on different floors.

### Task 2: Configure Appropriate Settings for Connectivity
**Objective**: Implement the following configurations:
- **VLANs**:
  - Create and assign VLANs for each department in Building A.
  - Ensure proper VLAN tagging and trunking for inter-VLAN routing.
  
- **DHCP**:
  - Configure DHCP on the main campus router to dynamically assign IP addresses to devices in Building A.
  
- **Routing**:
  - Set up RIPv2 on the routers to enable routing between different network segments within the university.
  - Implement static routing for the connection to the external email server hosted on the cloud.
  
- **Server Access**:
  - Ensure that internal servers in Building C and the external email server are accessible from all departments and faculties across both campuses.
  
- **End Device Configuration**:
  - Configure a few end devices with appropriate IP settings and test connectivity across the network.
  
**Verification**:
- Use `ping` and `tracert` commands to verify connectivity between devices in different buildings and campuses.
- Ensure that devices can reach both internal and external servers.

---

This plan and configuration should provide a robust and scalable network solution for Albio University, supporting all faculty, staff, and student needs across both campuses.
