---
name: network-triage
version: "1.1"
description: >
  Investigation procedures for AWS network connectivity failures
  including Transit Gateway routing issues, VPC security policy blocks,
  DNS failures, TLS/protocol faults, and IAM permission errors. Use this
  skill when investigating packet drops, TCP timeouts, VPC flow log
  REJECTs, or any connectivity failure across VPC, TGW, Cloud WAN, or
  Direct Connect.


# Network Triage

Stop at first confirmed fault. Always invoke before ad-hoc queries.

## OBSERVE (CloudWatch MCP)

- get_active_alarms, get_alarm_history: identify when the fault started.
- execute_log_insights_query on flow log groups: top-contributing subnet/ENI.

## ROUTE (Network MCP)

- find_ip_address (both IPs), get_eni_details, get_tgw_routes, get_cloudwan_routes:
  check BOTH directions. Missing return route is the leading cause.
- Evidence: ACCEPT at source, zero records at destination.

## POLICY (Network MCP + CloudTrail)

- SGs src+dst, NACLs inbound+outbound (stateless; return rules must be explicit),
  VPC Endpoint policies, Network Firewall rule groups.
- Evidence: REJECT at destination.

## DNS (Network MCP)

Prerequisite: ACCEPT both sides but application fails.

## LOAD BALANCER (Network MCP)

Prerequisite: ACCEPT both sides but application fails.

## PROTOCOL (PCAP Analyzer)

- analyze_tls_handshakes
- analyze_sni_mismatches
- analyze_tcp_retransmissions

## IDENTITY (IAM MCP)

## KNOWLEDGE (Bedrock KB)

## ESCALATE (Support MCP)

## OUTPUT

Fault layer → Evidence → CloudTrail attribution → CLI fix → Rollback

## SAFETY

Read-only. No production changes without human approval
---
