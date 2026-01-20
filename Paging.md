
1. Impact of paging policy provisioning

The provisioning of paging policy records and the sizing of tracking areas should be made with a full understanding of resulting impact to the message traffic level in the network.

While selective use of aggressive paging methods such as LastSeenTAI is necessary and appropriate, the overuse of aggressive paging methods can lead to overload conditions on both the CMM and eNB. While the CMM will react to mitigate the impact of overload conditions, the existence of these overload control mechanisms does not preclude the need to understand the consequences of choices when sizing tracking areas and provisioning of paging policy records. The paging channels of the eNBs are a limited resource. A well-designed paging policy will prioritize the use of MME paging so that the aggressive paging methods are utilized only in cases where less aggressive paging methods have failed to reach the UE or are used in limited cases where it is critical to reach the UE quickly (for example, for VoLTE calls).
The impact of paging policy provisioning can be demonstrated with the following general formula:

The factors of the equation can be described as follows:

Number of UEs served by MME
    The value indicates the number of registered UEs attached to a specific CMM MME.
Scenario frequency
    The value indicates how often a scenario that results in MME paging with that paging type will occur for a UE. This metric can be expressed in terms of events per second. However, it is expected that the event rate would be significantly less than one event per second. Please note that the scenario frequency will vary depending on time of day.
Average number of eNBs paged for paging type
    The value depends on the following:

        Number of paging attempts provisioned for paging type
        Paging method selected for each paging attempt
        Number of eNBs per TAI (if using LastSeenTAI or LastSeenTAINBTAI paging methods)
        UE movement patterns
        Paging success rate

The impact of use of a more aggressive paging method such as LastSeenTAI will depend greatly on factors such as:

    scenario frequency
    page attempt where this paging method is used
    number of eNBs associated with the last seen TAI

For example, use of LastSeenTAI paging for the second page attempt is normally not an issue because the UE is typically reached by the first page attempt. In addition, use of LastSeenTAI paging has been safely used on the first page attempt for rarely used paging types. However, it is suggested that service providers consider the use of LastSeenEnbList paging since this paging method generates a fraction of the paging message traffic while yielding a significantly better paging success rate than LastSeenENB.

The impact of the more aggressive paging methods such as LastSeenTAI is directly proportional to the average number of eNBs associated with tracking areas. If the number of eNBs per tracking area is limited to 60 to 80 (as per long-standing recommendations), then the impact of aggressive MME paging will be significantly less (for example, ~10%) than if tracking areas are configured with 600 to 800 eNBs. If larger tracking areas must be used in a network, then it is strongly recommended to employ aggressive paging methods for lesser used paging types or when less aggressive paging fails to reach the UE
The total page message traffic level would be calculated as follows:


           Total number of paging messages generated=
                (Number of paging messages generated for Basic paging)+
                (Number of paging messages generated for QCI 5 paging)+
                (Number of paging messages generated for SGS CS paging)+
                (Number of paging messages generated for SGS PS paging)+
                (Number of paging messages generated for other paging)

The CMM’s total paging message traffic level needs to be considered in conjunction with the CMM’s overall message transmission rate limit (for example, 500K messages per second on a CMM with a single pair of IPDS VMs). If most of a CMM’s message capacity is consumed with paging message traffic, overall MME call processing will suffer and the risk of overload is greatly increased. Furthermore, excessive MME paging affects the eNBs as well as the MME. If the MME generates excessive paging message traffic, the eNB’s paging channel can be overloaded and paging failures can result. In addition, an increase in the paging failure rate will typically lead to additional paging attempts and will lead to even more paging message traffic.
