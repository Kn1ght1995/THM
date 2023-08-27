# Box Name
<%*
  let title = tp.file.title
  if (title.startsWith("Untitled")) {
    title = await tp.system.prompt("Title");
    await tp.file.rename(title);
  } 
   
%>
## <%* tR += title %>

## Overview
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---

Enumeration Notes

---
## Initial Foothold
---

Initial Foothold Notes

---
## Privilege Escalation
----
