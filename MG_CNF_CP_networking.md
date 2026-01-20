CMG CNF CP networking
The internal and external network connections for the CMG Cloud Native Function CP, utilizing Kubernetes networks and Multus CNI plug-in for multiple interfaces. CMG CNF CP networking supports dual Data Switch Fabrics for redundancy and recommends pod-level VLAN tagging to reduce complexity.

CMG CNF networking (CP) shows the internal and external network connections on the CMG Cloud Native Function (CNF) CP. The internal network for OAM, MG, LB, and DB is enabled over a Kubernetes (K8s) network. The external network connection requires multiple interfaces on the LB function. These interfaces can be configured using the K8s Container Network Interface (CNI) plug-in Multus, which is supported for managing multiple interfaces that manage multiple K8s CNIs.

Figure: CMG CNF networking (CP)
