SentinelFlow

Multi-Agent AI-Powered Cybersecurity Incident Response Platform

SentinelFlow is a six-agent AI-powered Security Operations Center (SOC)
automation platform built as an internship capstone project. It
automates the incident-response lifecycle from security-alert ingestion
through threat intelligence, AI analysis, incident routing, human
review, and executive reporting.

Architecture

The system consists of six specialized agents orchestrated with n8n:

Alert Intake Agent --- receives, validates, normalizes, and
stores security alerts.

Threat Intelligence Agent --- enriches alerts using VirusTotal,
AbuseIPDB, and IPInfo.

Threat Analysis Agent --- uses a Groq-powered LLM to assess
risk, severity, TTPs, and blast radius.

Incident Routing Agent --- applies priority and routing logic
and records incident assignments.

Human Review Agent --- provides human-in-the-loop approval and
records analyst decisions.

Executive Reporting Agent --- generates executive-level security
reports using the reviewed incident data.

Agent Handoff Flow

Incoming Alert
      ↓
Agent 1 — Alert Intake
      ↓
Validated Alert
      ↓
Agent 2 — Threat Intelligence
      ↓
Enriched Threat Data
      ↓
Agent 3 — Threat Analysis
      ↓
AI Risk Assessment
      ↓
Agent 4 — Incident Routing
      ↓
Assigned Incident
      ↓
Agent 5 — Human Review
      ↓
Reviewed Incident
      ↓
Agent 6 — Executive Reporting
      ↓
Executive Security Report

Technology Stack

Technology                          Purpose

n8n                                 Workflow orchestration and agent
handoffs

Supabase                            Central database and shared context

Groq                                LLM-based threat analysis and
reporting

VirusTotal                          Threat-intelligence enrichment

AbuseIPDB                           IP reputation enrichment

IPInfo                              IP and network information

Discord                             Notifications and analyst
communication

Shared Context

Supabase provides the shared persistence layer for the six workflows.
The platform stores alert records, threat-intelligence results, AI
analysis, incident assignments, review status, executive reports, and
audit information.

Key Features

Six specialized AI/security agents

Automated alert ingestion

Threat-intelligence enrichment

LLM-based threat analysis

Incident prioritization and routing

Human-in-the-loop review

Executive security reporting

Shared database-backed context

Structured workflow handoffs

Error handling and validation

Discord notifications

Repository Structure

SentinelFlow/
├── workflows/
│   ├── workflow-01-Security Alert Collection.json
│   ├── workflow-02-Threat-Intelligence Enrichment.json
│   ├── workflow-03-AI Threat Classification.json
│   ├── workflow-04-incident-routing.json
│   ├── workflow-05-Analyst Review & Incident Closure.json
│   └── workflow-06-SOC Dashboard & Reporting.json
├── docs/
│   ├── architecture.png
│   └── workflow-documentation/

├── screenshots/

├── README.md
└── .gitignore

Setup

Prerequisites

n8n

Supabase project

Groq API key

VirusTotal API key

IPInfo API key

AbuseIPDB API key, if enabled

Discord configuration, if enabled

Installation

git clone https://github.com/dhaiv07/sentinelflow.git
cd sentinelflow

Import the six exported n8n workflow JSON files into n8n and configure
the required credentials.

Security

Do not commit API keys, passwords, webhook secrets, Supabase
service-role keys, or real security-alert data. Use n8n credentials or
environment variables for secrets.

Project Deliverables

Six n8n workflow exports

Workflow documentation

Agent architecture diagram

Project documentation

GitHub README

Screenshots and demo assets

Presentation

Demo video

Project Information

Project: SentinelFlow
Type: Internship Capstone Project
Domain: AI and Cybersecurity
Orchestration: n8n
Database: Supabase

Author

Dhairya Verma
