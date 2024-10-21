---
layout: base
title: "Programming a Beckhoff PLC for an oil separator system"
search: true
categories: 
  - PLC
last_modified_at: 2023-11-17
---

During my exchange semester in Norway at the University of Agder (UiA), I took the Industrial IT course. The course focused on the programming and concepts of PLCs, specifically Beckhoff PLCs. Together with three other students, we programmed the PLC of a three-phase oil separator as our final project. The goal of the project was to program a separator, to separate an emulsion of water and oil based on their difference in density. We developed the sequence of the whole system and implemented it using TwinCAT. We also designed a PID controller, which was simulated in MATLAB before implementation in the system. A HIM was added to control the system and finally the implemented controller was extensively tested and documented. The project was graded with an A, which corresponds to a 6.0 on the Swiss grading scale.

Schematics of oil separator:

![Schematics](/assets/image/IndustrialIT_UIA/Schematics.png)

User Human Machine Interface:

![HMI](/assets/image/IndustrialIT_UIA/HMI.png)

Physical Separator
![Separator](/assets/image/IndustrialIT_UIA/Physical_separator.jpeg)

**Read the full report [here](/assets/pdf/Industrial_IT_Project_Report.pdf)**
