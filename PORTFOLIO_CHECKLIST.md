# Portfolio Evidence Checklist

Use this checklist before changing a lab's status to **Complete**.

## Required Files

```text
lab-name/
├── README.md
├── packet-tracer/
│   └── lab-name.pkt
├── configs/
│   ├── R1-running-config.txt
│   └── SW1-running-config.txt
└── images/
    ├── topology.png
    ├── verification-before.png
    └── verification-after.png
```

Only include files produced by the actual lab. Remove passwords, secrets, public IP information, and unrelated device data before committing.

## README Requirements

- Business-style scenario, not only an exam objective
- Topology image
- Addressing and VLAN tables
- Stated requirements and expected traffic behavior
- Key configuration excerpts
- Exact verification commands
- Selected real command output
- At least one genuine failure and repair
- Root cause and post-fix proof
- Short explanation of the operational skill demonstrated

## Verification Minimum

Capture text output when possible so it is searchable. Screenshots should support—not replace—the text evidence.

```text
show ip interface brief
show interfaces trunk
show vlan brief
show spanning-tree
show etherchannel summary
show ip route
show ip ospf neighbor
show access-lists
show ip nat translations
show ip dhcp snooping binding
show ipv6 route
```

Use only the commands relevant to the lab. Include successful pings or traceroutes from representative endpoints.

## Troubleshooting Case Format

1. Ticket or symptom
2. Expected state
3. Initial evidence
4. Working hypothesis
5. Commands and observations
6. Root cause
7. Exact corrective change
8. Post-fix verification
9. Prevention or faster future check

## Screenshot Quality

- Crop to the relevant topology or output.
- Use readable zoom and consistent filenames.
- Do not expose passwords or personal information.
- Add a short caption explaining what the image proves.
- Prefer one strong topology image and a few targeted evidence images over a screenshot dump.

## Recruiter Test

A reviewer should understand within two minutes:

- what was built;
- what you personally configured;
- how you proved it worked;
- what broke;
- how you isolated the cause; and
- what the result says about your ability to support a real network.
