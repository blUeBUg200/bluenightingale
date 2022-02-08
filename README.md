# SECURITY-OPERATIONS - THE BLUE NIGHTINGALE

One of the biggest challenges in today's SOC is to identify a starting point for building a threat detection usecase library. The process described below is purely based on my understanding and interpretation of the referenced technologies / tools and how we can make use of them to streamline the entire process of usecase creation.

Requesting the audience to have the basic understanding of below topics before jumping into the process description,

[MITRE ATT&CK](https://attack.mitre.org/)

[DeTT&CT FRAMEWORK](https://github.com/rabobank-cdc/DeTTECT)

[TaHiTI Threat Hunting](https://www.betaalvereniging.nl/en/safety/tahiti/)

[CALDERA PAYLOADS](https://caldera.readthedocs.io/en/latest/index.html)

[SIGMA](https://github.com/SigmaHQ/sigma)


The briefing will primarily focus on how we can build a vendor neutral SOC team having a threat usecase library built over SIGMA. Going forward, we will use a generic name "BlueEngine" which refers back to the any of the SOC technologies such as Security Information and Event Management(SIEM), Network Detection and Response(NDR), Endpoint Detection and Response(EDR) which is in your current scope of work.

# THE PROCEDURE

When we start our hunting process for usecase creation, there are multiple factors / sources which can be the trigger point. I have classified them as below three broader categories

![image](https://user-images.githubusercontent.com/86832373/152847530-3ab37887-2f09-4edb-accf-cea3c8d0c553.png)

"**Log Sources**" : Gathering information about your existing "BlueEngine" integrated devices and creating usecase based on the data ingested. The data can be of any forms such as "Event Logs", "Network Flows", etc., [more easiest way.. isn't it ?]

"**Threat Intelligence**" : Gathering information about the TTP of an attacker / group as listed in Government / Non-Government Threat Inteliigence reports, Internal forensics reports, any recent IR reports, etc.,

"**Red Teaming / Penetration Testing**" : Recent Red Teaming / Penetration Testing reports which helps to narrow down our weakest area and focus on enhancing detections around the "Tactics", "Techniques" or "Sub-Techniques". which was exploited without being detected by Blue team.

# **PHASE I : LOG SOURCES**

Below is the generic outline of PHASE I,

![image](https://user-images.githubusercontent.com/86832373/152855379-a103cb13-0376-434a-89f3-3d439c7dc321.png)

> S2 is the convergence point in all three phases [P1, P2, P3]

## PICK A MITRE DATA SOURCE
Our goal in this phase is to identify the data source to focus. MITRE ATT&CK ["Data Sources"](https://attack.mitre.org/datasources/) provides the list of data sources and by doing some homework, we can understand the importance of data sources and their major role in creating a usecase library. Having both [MITRE ATT&CK](https://attack.mitre.org/) & [DeTT&CT FRAMEWORK](https://github.com/rabobank-cdc/DeTTECT) in hand, I was able to indetify the number of "Techniques" and "Sub-Techniques" which can be covered by each data source. Screenshot as provided in [DeTT&CT FRAMEWORK](https://github.com/rabobank-cdc/DeTTECT) github [higher the number, more brighter your detection matrix...]

![image](https://user-images.githubusercontent.com/86832373/152853649-cf9b3f17-1344-4c1a-a43a-96f663979be5.png)

_[data trimmed to show only top 6]_

In addition to the above, below data points need to be considered while finalizing your data source to work with,

![image](https://user-images.githubusercontent.com/86832373/152856722-0dce957a-88e3-4af4-aa1d-1c6f89883ff8.png)
more details about the table can be found in [DeTT&CT FRAMEWORK](https://github.com/rabobank-cdc/DeTTECT)

I would recommend you to prioritize below data sources since the associated information might already be available or easy to ingest into your "BlueEngine". 

1. Command : Command Execution (ex: Windows EID 4688 of cmd.exe showing command-line parameters, ~/.bash_history, or ~/.zsh_history)
2. Process : Process Creation (Birth of a new running process (ex: Windows EID 4688)
3. Network Traffic : Flow (Summarized network packet data, with metrics, such as protocol headers and volume (ex: Netflow or Zeek http.log)

## COMPILE MITRE MATRICES

Consider I have choosen to start with "Command : Command Execution" for windows platform. It would be great if I could see what "Techniques", "Sub-Techniques" does this cover in a MITRE map, isn't it ? Yes, we can leverage the functionality of [DeTT&CT FRAMEWORK](https://github.com/rabobank-cdc/DeTTECT) to show us the [matrix](https://github.com/BlueBee-LLC/SECURITY-OPERATIONS/blob/main/Command_Command_Execution_MITRE.svg)

## TRIGGER TaHiTI

FS-ISAC TaHiTI [excelbook](https://www.betaalvereniging.nl/wp-content/uploads/Magma-for-Threat-Hunting.xlsx) is designed in such way it maps the usecase to "Kill Chain" phases. Wherein in our case, we will align them to MITRE ATT&CK. Below changes to be performed,

L1 Sheet - MITRE TACTICS

L2 Sheet - MITRE TECHNIQUES

L3 Sheet - Specific usecases relating directly to technique or sub-technique associated with the tagged technique


We have identified the logsource to focus (Command : Command Execution), we have the compiled MITRE matrix, we have tweaked the TaHiTI workbook, so what's next ? Create usecase ??....nahhhhh !!! Hunt before creating usecase ??....Yesss.

![image](https://user-images.githubusercontent.com/86832373/152861322-cd70207d-78ec-4aed-bc47-6a7552448e23.png)

Consider you focus on creating usecase for "Execution" - Tactic and its associated 5 "Techniques" as per the compiled matrix from previous step. Start the threat hunting process and fill in all required fields in L3 sheet of TaHiTI workbook and complete the hunting process for a given Technique or Sub-Techbique.

## BUILD SIGMA

Now we came to the final part of PHASE I usecase creation. Once hunt is completed, we will create an associated sigma considering the "false positive" factor as well for each sigma rule. Considering the below facts, the rule will either proposed as an usecase to be deployed in your "BlueEngine" or it remains as a hunt book,

- The number possible FP it can generate based the techniques nature -- example: [T1059.006](https://attack.mitre.org/techniques/T1059/006/)
- Based on the observation out from TaHiTI output

> LOOKOUTS

- Sigma conf file should be modified accordingly to match your index / field values. Yes, the conf files are super flexible to have this modifications done.

### STAY TUNED FOR P2 and P3 PHASES...
