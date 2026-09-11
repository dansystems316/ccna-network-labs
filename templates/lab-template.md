# Lab Title

**Status:** In progress  
**Platform:** Cisco Packet Tracer  
**Skills:** Add three to six searchable skills

## Scenario

Describe the organization, user need, or support ticket this lab models. Explain why the network change matters.

## Objective

State the measurable result of the lab.

## Topology

![Topology](images/topology.png)

Describe device roles and important links. Do not mark the lab complete until the image is real.

## Addressing and VLAN Plan

| Device | Interface | IPv4/IPv6 address | VLAN | Default gateway | Purpose |
|---|---|---|---:|---|---|
| R1 | G0/0 | | | | |

## Requirements

- Requirement 1
- Requirement 2
- Requirement 3

## Files

- [Packet Tracer lab](packet-tracer/lab-name.pkt)
- [Device configurations](configs/)

Add these links only after the files exist.

## Implementation

Explain the design decisions briefly, then include the important configuration—not unexplained configuration dumps.

### R1

```text
enable
configure terminal
!
! Relevant configuration
!
end
write memory
```

## Verification Plan

| Test | Expected result | Evidence |
|---|---|---|
| Source host to gateway | Successful ping | Command output or screenshot |
| Required remote traffic | Permitted | Output |
| Prohibited traffic | Blocked | ACL counter or test |
| Control-plane state | Correct neighbor/port/route state | Show command |

## Verification Output

Record selected real output from the completed lab.

```text
show ip interface brief
show ip route
```

## Troubleshooting Case

### Symptom

Describe exactly what failed.

### Expected State

Describe what should have happened.

### Evidence and Reasoning

| Step | Command/test | Observation | Conclusion |
|---:|---|---|---|
| 1 | | | |

### Root Cause

State the confirmed cause. Do not list guesses.

### Resolution

Show the exact change that corrected the problem.

### Post-Fix Proof

Repeat the original failing test and capture the successful result.

## What I Learned

Summarize the technical lesson and how you would diagnose the issue faster in a production support setting.
