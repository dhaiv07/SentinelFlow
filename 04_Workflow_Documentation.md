Workflow 1 – Security Alert Collection
4.1.1 Purpose

The Security Alert Collection workflow is responsible for receiving incoming cybersecurity alerts and preparing them for further investigation. It acts as the entry point of the SentinelFlow system by collecting alert information, validating the received data, and storing the alert in the central database.

4.1.2 Trigger

The workflow is triggered when a new security alert is received through the configured n8n trigger/webhook.

4.1.3 Input

The workflow accepts structured security-alert information such as:

Alert source
Alert type
Severity
Source IP or other indicators
Timestamp
Alert description
Additional metadata

4.1.4 Workflow Process
Receive the incoming security alert.
Validate the received alert data.
Extract and normalize the relevant alert fields.
Generate or maintain the required incident/alert information.
Store the alert in Supabase.
Return a successful response after the alert is processed.
Make the stored alert available for the Threat Intelligence Enrichment workflow.

4.1.5 n8n Nodes Used
Webhook/Trigger node
Code/JavaScript nodes for data processing and validation
Supabase database nodes
Conditional/logic nodes where required
Response node

4.1.6 Output

The workflow produces a validated and structured security-alert record containing the information required for subsequent threat-intelligence processing.

4.1.7 Error Handling

The workflow handles invalid or incomplete alert data through validation and conditional processing. Database or processing failures are prevented from being treated as successful executions, allowing unsuccessful alert processing to be identified and investigated.

4.2 Workflow 2 – Threat Intelligence Enrichment
4.2.1 Purpose

The Threat Intelligence Enrichment workflow adds external security context to the collected alert. It retrieves information about relevant indicators, such as IP addresses, from threat-intelligence and IP-information services.

4.2.2 Trigger

The workflow is triggered when a validated security alert becomes available from the previous stage.

4.2.3 Input

The workflow receives the validated alert and relevant indicators extracted from it, including:

Alert ID
Source IP
Alert type
Severity
Other available indicators

4.2.4 Workflow Process
Retrieve the latest/required alert information.
Extract the relevant threat indicators.
Send indicators to the configured external intelligence services.
Collect the returned intelligence.
Combine the enrichment results with the original alert information.
Store the enriched information in Supabase.
Pass the enriched threat data to the AI Threat Classification workflow.
4.2.5 n8n Nodes Used
Trigger/Webhook node
Supabase nodes
HTTP Request nodes
VirusTotal integration
AbuseIPDB integration, where configured
IPInfo integration
Code/JavaScript nodes
Merge/processing nodes
Conditional nodes

4.2.6 Output

The workflow produces an enriched security-alert record containing the original alert information together with external threat-intelligence results.

4.2.7 Error Handling

API failures, unavailable intelligence results, malformed responses, and missing indicators are handled through conditional processing and workflow error handling. Failed enrichment does not automatically imply that the original alert is invalid; the available information can still be retained for subsequent analysis.

4.3 Workflow 3 – AI Threat Classification
4.3.1 Purpose

The AI Threat Classification workflow analyzes the enriched security alert using an LLM and produces a structured assessment of the potential threat and associated risk.

4.3.2 Trigger

The workflow is triggered after the threat-intelligence enrichment stage has produced the required enriched alert data.

4.3.3 Input

The workflow receives:

Alert information
Threat-intelligence results
Indicator information
Severity information
Relevant contextual data stored for the incident

4.3.4 Workflow Process
Retrieve the enriched incident information.
Prepare the relevant information for AI analysis.
Send the incident context to the configured LLM.
Analyze the alert and available threat intelligence.
Generate a structured threat/risk assessment.
Store the AI classification and analysis results.
Pass the assessment to the Incident Assignment & Escalation workflow.

4.3.5 n8n Nodes Used
Trigger/Webhook node
Supabase nodes
Data transformation/Code nodes
LLM/AI Agent node
Prompt/structured-output processing
Conditional/logic nodes

4.3.6 Output

The workflow produces a structured AI threat assessment containing relevant classification and risk information used by the incident-routing stage.

4.3.7 Error Handling

The workflow handles missing incident context, unsuccessful AI responses, malformed structured output, and processing failures through validation and conditional logic. Invalid AI output is not treated as a successful classification.

4.4 Workflow 4 – Incident Assignment & Escalation
4.4.1 Purpose

The Incident Assignment & Escalation workflow determines the appropriate response path for an analyzed security incident. It uses the AI-generated assessment and incident information to determine whether escalation or further review is required.

4.4.2 Trigger

The workflow is triggered when the AI Threat Classification stage produces a completed threat assessment.

4.4.3 Input

The workflow receives:

Alert information
Threat-intelligence information
AI threat classification
Risk/severity information
Incident metadata

4.4.4 Workflow Process
Retrieve the analyzed incident.
Evaluate the AI-generated risk assessment.
Apply incident-routing and escalation conditions.
Determine the appropriate response path.
Assign or escalate the incident as required.
Store the updated incident status and assignment.
Notify the appropriate response channel where configured.
Pass incidents requiring review to the Analyst Review workflow.

4.4.5 n8n Nodes Used
Trigger/Webhook node
Supabase nodes
Code/JavaScript nodes
IF/conditional nodes
Assignment/routing logic
Notification nodes
Data transformation nodes

4.4.6 Output

The workflow produces an assigned or escalated incident containing its routing decision, priority, and updated status.

4.4.7 Error Handling

Missing analysis data, invalid routing conditions, database failures, and notification failures are handled through validation and conditional processing. Incidents are not marked as successfully assigned unless the required processing has completed.

4.5 Workflow 5 – Analyst Review & Incident Closure
4.5.1 Purpose

The Analyst Review & Incident Closure workflow provides a human-in-the-loop stage where an analyst can review the AI-generated assessment and incident information before the incident is finalized.

4.5.2 Trigger

The workflow is triggered when an incident is routed for analyst review.

4.5.3 Input

The workflow receives:

Incident information
Alert details
Threat-intelligence results
AI threat assessment
Routing decision
Relevant incident context

4.5.4 Workflow Process
Retrieve the incident requiring review.
Present the relevant incident information for analyst review.
Obtain the analyst's decision/approval.
Record the analyst decision.
Update the incident status.
Mark the incident as reviewed or closed according to the review outcome.
Store the final review information in Supabase.
Pass the finalized incident information to the Analytics & Reporting workflow.

4.5.5 n8n Nodes Used
Trigger/Webhook node
Supabase nodes
Human approval/review mechanism
Code/JavaScript nodes
Conditional/IF nodes
Status-update nodes
Notification nodes where configured

4.5.6 Output

The workflow produces a reviewed incident containing the analyst's decision, review status, and updated incident state.

4.5.7 Error Handling

The workflow accounts for incomplete review information, invalid approval states, missing incidents, and database/update failures. An incident is not considered successfully reviewed until the analyst decision and resulting status have been recorded.

4.6 Workflow 6 – Analytics & Reporting
4.6.1 Purpose

The Analytics & Reporting workflow consolidates the finalized incident information and produces a higher-level security report for monitoring and executive visibility.

4.6.2 Trigger

The workflow is triggered when the reviewed/finalized incident becomes available for reporting.

4.6.3 Input

The workflow receives:

Final incident information
Original security alert
Threat-intelligence results
AI threat assessment
Incident assignment information
Analyst review decision
Incident status

4.6.4 Workflow Process
Retrieve the latest finalized incident information.
Collect the relevant incident and analysis data.
Consolidate the information into a reporting context.
Generate the required security/management summary.
Store the resulting report and relevant analytics information.
Make the final information available for monitoring and executive reporting.

4.6.5 n8n Nodes Used
Trigger/Webhook node
Supabase nodes
Data retrieval nodes
Code/JavaScript nodes
LLM/AI processing nodes where configured
Data transformation nodes
Reporting/output nodes

4.6.6 Output

The workflow produces a consolidated security report containing the incident summary, analysis, response information, review outcome, and relevant reporting information.

4.6.7 Error Handling

The workflow validates the availability of the required incident data before generating the report. Missing records, incomplete information, AI/report-generation failures, and database errors are handled through conditional processing and error handling to prevent incomplete reports from being treated as successful outputs.
