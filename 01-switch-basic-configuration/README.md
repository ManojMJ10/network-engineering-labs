# Lab 01 – Cisco Switch Basic Configuration

## Objective

Learn how to access and configure a Cisco switch using the IOS Command Line Interface, secure privileged access, and save the configuration.

## Tools Used

- Cisco Packet Tracer
- Cisco Switch
- Cisco IOS CLI

## Configuration Commands

```bash
enable
configure terminal
hostname SW1
enable secret CCNA2026
service password-encryption
end
copy running-config startup-config
```

## Verification

```bash
show running-config
show startup-config
```

## Troubleshooting Experience

During the lab, I encountered an enable secret password issue that prevented access to privileged EXEC mode.

In Cisco Packet Tracer, I exported the running configuration, updated the enable secret in a copy, and merged the changes.

I then verified privileged access and practiced changing the enable secret through the CLI.

## Skills Learned

- Cisco IOS command modes
- Hostname configuration
- Enable secret password configuration
- Password encryption
- Configuration backup and recovery
- Saving running configuration to startup configuration

## Conclusion

This lab improved my understanding of Cisco switch administration, basic security configuration, and troubleshooting.

