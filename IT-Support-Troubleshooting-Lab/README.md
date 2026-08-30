# IT Support Troubleshooting Lab

A collection of simulated IT support tickets focused on practical troubleshooting, documentation, and verification.

## Lab Environment

The scenarios in this repository are performed in **NetSim**, a browser-based network simulator that supports Linux-style networking commands. These are simulated support scenarios created for hands-on practice and portfolio documentation; they are not presented as real customer incidents.

## Tickets

| Ticket | Scenario | Status |
|---|---|---|
| [Ticket 01](Ticket-01-Incorrect-IP-Configuration/) | Workstation cannot reach its default gateway because of an incorrect static IP address | Resolved |
| [Ticket 02](Ticket-02-DNS-Name-Resolution-Failure/) | Workstation can reach a server by IP but cannot resolve its hostname because of an incorrect DNS server | Resolved |
| [Ticket 03](Ticket-03-Web-Service-Unavailable) | Server remains reachable and DNS resolves correctly, but the company website is unavailable because the HTTP service is not running | Resolved |
| [Ticket 04](./Ticket-04-Multiple-Network-and-Web-Service-Issues/) | Workstation cannot access the company website due to an incorrect default gateway and misconfigured HTTP service port | Resolved |

## Troubleshooting Approach

Each ticket follows the same basic workflow:

1. Identify the reported symptom.
2. Gather evidence before making changes.
3. Test connectivity and inspect configuration.
4. Determine the root cause.
5. Apply the smallest appropriate fix.
6. Verify that connectivity or service has been restored.

More simulated tickets will be added as the lab progresses.
