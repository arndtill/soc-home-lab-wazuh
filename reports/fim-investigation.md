# File Integrity Monitoring Investigation

## 1. Objective

Verify that Wazuh detects file creation, modification and deletion on a monitored Windows Endpoint.

## 2. Test Environment

| Field | Value |
|---|---|
| Agent | Windows10Pro |
| Operating System | Windows 10 Pro |
| Monitored Directory | c:\Wazuh-Lab |
| Test File | fim-test.txt |
| Monitoring Mode | Real-time |
| Test Date | November 7, 2026 |

## 3. Test Scenario 

Three controlled file operations were performed within the monitored directory:

1. Creation of a test file
2. Modification of the file contents
3. Deletion of the file

Each operation was performed to verify the corresponding FIM detection.

## 4. Detection Results

| Activity | Rule ID | Alert Level | Description |
|--|---|---|---|
| File creation | 554 | 5 | File added to the system |
| File modification | 550 | 7 | Integrity checksum changed |
| File deletion | 553 | 7 | File deleted |

All three operations generated the expected alerts.

## 5. Investigation

The alerts were located through the Threat Hunting section of the Wazuh Dashboard using the following query:

agent.name:Windows10Pro AND rule.id: (550 OR 553 OR 554)

The event details were inspected to identify the affected file, event type and available file hashes.

| Field | Purpose |
|---|---|
| rule.id | Identifies the detection rule |
| rule.description | Describes the detected activity |
| syscheck.path | Identifies the monitored file |
| syscheck.event | Identifies the type of file operation |
| syscheck.mode | Identifies the monitoring mode |
| syscheck.sha256_before | Records the previous SHA-256 hash, when available |
| syscheck.sha256_after | Records the updated or last known SHA-256 hash |
| syscheck.size_before | Records the previous file size, when available |
| syscheck.size_after | Records the updated or last known file size |

## 5.1 File Creation

Wazuh detected the creation of fim-test.txt through rule 554.

| Field | Value |
|---|---|
| Event | added |
| Monitoring Mode | realtime |
| File Size | 17 bytes |
| SHA-256 After | 7a32d9037fb7ee4cf70907f38b68dbd5e76c21502574ea3d0044dfd39349915117 |

This event established the initial recorded state of the file.

## 5.2 File Modification

Wazuh detected a change in the file contents through rule 550.

| Field | Value |
|---|---|
| Event | modified |
| Monitoring Mode | realtime |
| Size Before | 17 bytes |
| Size After | 31 bytes |
| SHA-256 Before | 7a32d9037fb7ee4cf70907f38b68dbd5e76c21502574ea3d0044dfd393499151 |
| SHA-256 After | 27be96e728be34dc015d79a3b40482c6e6f5c47689b4308e746e9429d739ed3e |

The previous SHA-256 hash matches the hash recorded during file creation. The change in both file size and hash confirms that the file contents were modified.

## 5.3 File Deletion

Wazuh detected the deletion of the monitored file through rule 553.

| Field | Value |
|---|---|
| Event | deleted |
| Monitoring Mode | realtime |
| Last Recorded Size | 31 bytes |
| SHA-256 After | 27be96e728be34dc015d79a3b40482c6e6f5c47689b4308e746e9429d739ed3e |

The deletion alert retains the last known SHA-256 hash and file size in the corresponding after fields. The recorded hash matches the hash obtained after the file modification.

## 5.4 Findings

- All three alerts reference the same file path and Endpoint
- All three events were detected in real time
- The file size increased from 17 to 31 bytes
- The SHA-256 hash changed after the file modification
- The deletion alert retained the last recorded hash and file size

These findings establish a consistent sequence of file creation, modification and deletion.

## 6. Event Timeline

| Time (UTC-3) | Activity | Rule ID |
|---|---|---|
| 11:53:00 | File created | 554 |
| 11:54:54 | File modified | 550 |
| 11:57:05 | File deleted | 553 |

## 7. Alert Classification

- **Classification:** benign true positive
- **Malicious activity:** not identified
- **Response:** no remediation required

The detected file operations were intentionally performed as part of the controlled test.

## 8. Evidence

- [Threat Hunting Search](../evidence/fim/01_fim_search.PNG)
- [File Creation Alert](../evidence/fim/02_fim_file_added.PNG)
- [File Modification Alert](../evidence/fim/03_fim_file_modified.PNG)
- [File Deletion Alert](../evidence/fim/04_fim_file_deleted.PNG)

## 9. Conclusion

Wazuh successfully detected all three file operations and made the corresponding alerts available for investigation through the Dashboard. By comparing file hashes, file sizes and event timestamps, the sequence of file operations was reconstructed. The exercise demonstrated real-time file integrity monitoring, rule-based detection, alert filtering and security event analysis.

## 10. Limitations

The investigation was limited to a dedicated test directory and controlled file operations. File integrity monitoring detects changes but does not independently determine whether those changes are malicious.
