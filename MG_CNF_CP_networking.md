####CMG CNF CP networking


The internal and external network connections for the CMG Cloud Native Function CP, utilizing Kubernetes networks and Multus CNI plug-in for multiple interfaces. CMG CNF CP networking supports dual Data Switch Fabrics for redundancy and recommends pod-level VLAN tagging to reduce complexity.

CMG CNF networking (CP) shows the internal and external network connections on the CMG Cloud Native Function (CNF) CP. The internal network for OAM, MG, LB, and DB is enabled over a Kubernetes (K8s) network. The external network connection requires multiple interfaces on the LB function. These interfaces can be configured using the K8s Container Network Interface (CNI) plug-in Multus, which is supported for managing multiple interfaces that manage multiple K8s CNIs.

<img width="624" height="380" alt="image" src="https://github.com/user-attachments/assets/009300dd-9fd6-4146-9b6d-5ea6ee4b9ed7" />


For redundancy, dual Data Switch Fabric (DSF)s are supported from the application side.

Nokia recommends using pod-level VLAN tagging (instead of host-level) to reduce the number of required interfaces on the pod-level and avoid complexity when assigning interfaces.

To ensure that dual DSF have Single-Root I/O virtualization (SR-IOV) interfaces allocated from different physical Network Interface Card (NIC) ports and dual external int
