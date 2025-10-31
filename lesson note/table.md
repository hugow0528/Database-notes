

**`Student` Table**
| StudentID | Name | Class | JoinedOn |
| :--- | :--- | :--- | :--- |
| ST1001 | Alice Wong | S3A | 2024-09-02 |
| ST1002 | Ben Chan | S3B | 2024-09-02 |
| ST1003 | Cathy Lee | S4A | 2024-09-03 |
| ST1004 | David Lam | S5C | 2024-08-30 |
| ST1005 | Emily Ng | S2A | 2024-09-04 |
| ST1006 | Frank Ho | S5A | 2024-09-01 |
| ST1007 | Grace Cheung| S2B | 2024-09-05 |
| ST1008 | Henry Lee | S6A | 2024-08-29 |
| ST1009 | Ivy Tang | S4B | 2024-09-03 |
| ST1010 | Jack Lau | S3C | 2024-09-02 |

**`SportEvent` Table**
| EventID | EventName | Category | EventDate | Location |
| :--- | :--- | :--- | :--- | :--- |
| E3001 | 100m Sprint | Track | 2024-10-12 | Main Stadium |
| E3002 | 200m Sprint | Track | 2024-10-12 | Main Stadium |
| E3003 | 4x100m Relay | Track | 2024-10-13 | Main Stadium |
| E3004 | Long Jump | Field | 2024-10-14 | Field A |
| E3005 | High Jump | Field | 2024-10-14 | Field A |
| E3006 | Shot Put | Field | 2024-10-15 | Field B |
| E3007 | Basketball 3v3 | Ball | 2024-10-16 | Gym Court 1|
| E3008 | Table Tennis Singles| Ball | 2024-10-16 | Hall |
| E3009 | Badminton Doubles | Ball | 2024-10-17 | Hall |
| E3010 | Swimming 50m Freestyle | Aquatic | 2024-10-18 | Aquatic Center|
| E3011 | Swimming 100m Breaststroke| Aquatic | 2024-10-18 | Aquatic Center|
| E3012 | Cross Country 5km | Track | 2024-10-20 | Country Park|

**`Enrolled` Table**
| StudentID | EventID | RegisteredOn |
| :--- | :--- | :--- |
| ST1001 | E3001 | 2024-09-20 |
| ST1001 | E3003 | 2024-09-22 |
| ST1002 | E3002 | 2024-09-21 |
| ST1002 | E3007 | 2024-09-25 |
| ST1003 | E3004 | 2024-09-23 |
| ST1003 | E3010 | 2024-09-29 |
| ST1004 | E3006 | 2024-09-24 |
| ST1005 | E3008 | 2024-09-26 |
| ST1005 | E3009 | 2024-09-28 |
| ST1006 | E3011 | 2024-09-27 |
| ST1006 | E3005 | 2024-10-01 |
| ST1007 | E3003 | 2024-09-30 |
| ST1008 | E3010 | 2024-10-02 |
| ST1008 | E3012 | 2024-10-05 |
| ST1009 | E3001 | 2024-10-03 |
| ST1010 | E3002 | 2024-10-04 |
| ST1010 | E3003 | 2024-10-06 |
| ST1010 | E3007 | 2024-10-07 |
