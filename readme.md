# AMK Racing Kit Configuration - Zips Racing 2025
Project configuration and documentation for the AMK racing kit. This project mainly customizes the CAN interface, switching it to use the free CAN configuration with torque setpoints rather than speed setpoints. Outside this there isn't much different between this and the template projects (CAN ID, derating values, etc.).

## CAN IDs
Each inverter uses the same CAN configuration, but with different message message IDs. The configuration described below is the generic configuration, each ID is given as an offset from the node's base ID (Ex base 0x200 + offset 0x300 => 0x500).

| Inverter    | Base ID |
|-------------|---------|
| Rear Left   | 0x200   |
| Rear Right  | 0x201   |
| Front Left  | 0x202   |
| Front Right | 0x203   |

## CAN Configuration

| Message           | Message Name      | ID Offset | Signal Name              | Datatype | Type    | Index | Bytes  |
|-------------------|-------------------|-----------|--------------------------|----------|---------|-------|--------|
| Send Message 1    | Motor Feedback    | 0x004     | Status word              | UNS16    | Special | 3     | [0, 1] |
|                   |                   |           | Actual torque value      | SGN16    | Special | 19    | [2, 3] |
|                   |                   |           | Actual speed value       | SGN32    | Special | 20    | [4, 7] |
| Send Message 2    | Power Consumption | 0x008     | DC bus voltage           | UNS16    | SERCOS  | 32836 | [0, 1] |
|                   |                   |           | Torque current feedback  | SGN16    | SERCOS  | 32834 | [2, 3] |
|                   |                   |           | Actual power value       | UNS32    | SERCOS  | 33100 | [4, 7] |
| Send Message 3    | Temperatures      | 0x300     | Temperature internal     | SGN16    | SERCOS  | 33116 | [0, 1] |
|                   |                   |           | Temperature external     | SGN16    | SERCOS  | 33117 | [2, 3] |
|                   |                   |           | Temperature sensor motor | UNS16    | SERCOS  | 34166 | [4, 5] |
|                   |                   |           | IGBT temperature         | SGN16    | Special | 27    | [6, 7] |
| Send Message 4    | Errors 1          | 0x304     | Diagnostic number        | UNS32    | Special | 21    | [0, 3] |
|                   |                   |           | Error info 1             | UNS32    | Special | 22    | [4, 7] |
| Send Message 5    | Errors 2          | 0x308     | Error info 2             | UNS32    | Special | 23    | [0, 3] |
|                   |                   |           | Error info 3             | UNS32    | Special | 24    | [4, 7] |
| Receive Message 1 | Motor Request     | 0x000     | Control word             | UNS16    | Special | 4     | [0, 1] |
|                   |                   |           | Torque setpoint          | SGN16    | Special | 17    | [2, 3] |
|                   |                   |           | Positive torque limit    | SGN16    | Special | 13    | [4, 5] |
|                   |                   |           | Negative torque limit    | SGN16    | Special | 14    | [6, 7] |