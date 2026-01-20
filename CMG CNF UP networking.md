###CMG CNF UP networking

The networking details the internal and external network connections for Cloud Native Function UP, utilizing Kubernetes for internal functions and Multus CNI for managing multiple interfaces. The same restrictions for Data Switch Fabric and external network interface assignments apply as in CMG CNF networking CP.



<img width="624" height="380" alt="image" src="https://github.com/user-attachments/assets/d9dc7a2e-3199-4e4a-bf87-8dc73079b79c" />

CMG CNF networking (UP) shows the internal and external network connections on a CMG Cloud Native Function (CNF) UP. The internal network for the OAM, MG, LB, and DB functions is enabled over a network provided by Kubernetes (K8s). The external network connection requires multiple interfaces on the LB function and MG function (for LB-less deployment). These can be configured using the K8s Container Network Interface (CNI) plug-in Multus, which is supported for managing multiple interfaces that manage multiple K8s CNIs.
