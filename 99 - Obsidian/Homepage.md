---
banner: "https://images8.alphacoders.com/973/973575.jpg"
banner_y: 0.60333
banner_x: 0.5
cssclasses:
  - callouts-outlined
---
:LiList: Menu
---

> [!multi-column]
> 
> > [!todo] Daily
> > ```dataview
> > LIST WITHOUT ID file.link 
> > FROM #daily
> > SORT date(regexreplace(file.name, "\\s.*$", ""), "dd.MM.yyyy") DESC
> > LIMIT 4
> > ```
> 
> > [!check] Recent file update
> > ```dataview
> > LIST WITHOUT ID file.link 
> > SORT file.mtime DESC
> > LIMIT 4
> > ```
> 
> > [!favorite] Favorite
> > ```dataview
> > LIST WITHOUT ID file.link 
> > FROM #favorite
> > SORT file.mtime
> > LIMIT 4
> > ```

<br>

---
:LiCheckCircle: Todo
---

> [!multi-column]
> > ### :LiClipboardList: Today
> > ```tasks
> > not done
> > short mode
> > due today
> > ```
> 
> > ### :LiCalendarDays: Tomorrow
> > ```tasks
> > not done
> > short mode
> > due tomorrow
> > ```
> 
> > ### :LiCalendarDays: Next 5 days
> > ```tasks
> > not done
> > short mode
> > (due after tomorrow) AND (due before in 8 days)
> > ```

<br>

---

:LiListTodo: All Tasks
---
![[Task Manager.base]]