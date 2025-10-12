---
tags:
  - daily
---

---

<%* 
let rawDate = moment(tp.file.title, "DD.MM.YYYY (dddd)"); 
let dailyPlansFolder = "02 - Areas/Planning/Daily plans/"; 
let dailyPlansFormat = "YYYY/MM - MMMM/DD.MM.YYYY (dddd)"; 
let yesterday = dailyPlansFolder + rawDate.subtract(1, "day").format(dailyPlansFormat); 
let tomorrow = dailyPlansFolder + rawDate.add(2, "day").format(dailyPlansFormat); 
%>
#### <% "[[" + yesterday + "|< Yesterday]]" %> - <% "[[" + tomorrow + "|Tomorrow >]]" %>

---

<br>

> [!multi-column]
<%-* if (!tp.file.title.includes("Sunday")) { %>
> 
> > [!regtasks]+ Regular tasks
> > - [ ] :LiPersonStanding: Morning exercises
> > - [ ] :LiShowerHead: Contrast shower
> > - [ ] :LiKeyboard: Blind typing
<%-* } %>
<%-* if (tp.file.title.includes("Wednesday") || tp.file.title.includes("Saturday")) { %>
> > - [ ] :LiPaintbrush: CleanUp
<%-* } %>
<%-* if (tp.file.title.includes("Tuesday") || tp.file.title.includes("Thursday") || tp.file.title.includes("Saturday")) { %>
> > - [ ] :LiBicepsFlexed: Sports
<%-* } %>
> 
> > [!tasks]+ Today's tasks
> > - [ ] #todo 
> > - [ ] #todo 
> > - [ ] #todo 

<br>

---

### All Tasks
![[Task Manager.base]]