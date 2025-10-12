# Obsidian Vault Template

A comprehensive Obsidian vault template designed for personal knowledge management, task tracking, and media cataloging. This template provides a structured approach to organizing your digital life with powerful automation and visualization features.

## 🌟 Key Features

### 📱 Homepage Dashboard

![Homepage](99%20-%20Obsidian/Attachments/README/Homepage.png)

The central hub of your vault (`99 - Obsidian/Homepage.md`) provides quick access to:

- **Recent Daily Notes**: Quick navigation to your last 4 daily entries
- **Recently Updated Files**: Stay on top of your latest work
- **Favorite Notes**: Pin your most important content
- **Task Overview**: See today's tasks, tomorrow's tasks, and upcoming tasks for the next 5 days
- **Full Task Manager**: Embedded view of all tasks across categories

### 📅 Daily Notes System

![Daily Note](99%20-%20Obsidian/Attachments/README/Daily.png)

Automated daily note creation with:

- **Smart Date Navigation**: Links to previous and next days
- **Regular Tasks**: Customizable recurring tasks based on day of week
    - Morning exercises, contrast shower, blind typing (most days)
    - CleanUp tasks (Wednesday, Saturday)
    - Sports activities (Tuesday, Thursday, Saturday)
- **Today's Tasks**: Quick task entry with #todo tags
- **Integrated Task Manager**: Full task overview embedded in each daily note

Daily notes are organized hierarchically: `02 - Areas/Planning/Daily plans/YYYY/MM - MMMM/DD.MM.YYYY (dddd)`

### ✅ Task Manager

![Task Manager](99%20-%20Obsidian/Attachments/README/TaskManager.png)

A powerful task management system built with Obsidian Bases:

**Task Properties:**

- **Status**: 📝 To Do, 🔄 In Progress, ✅ Done, ❌ Cancelled
- **Priority**: 🔥 Highest, 🔴 High, 🟠 Medium, 🟡 Low, 🟢 Lowest
- **Difficulty**: 🔴 Hard, 🟠 Medium, 🟢 Easy
- **Category**: 💼 For work, 🎓 University, 🌟 Wants, 🟣 Obsidian, 👥 Relationship
- **Due Date**: Flexible date tracking

**Task Views:** The Task Manager includes filtered views for each category:

- 📋 All tasks
- 💼 For work
- 🎓 University
- 🌟 Wants
- 🟣 Obsidian

**Quick Task Creation:** Tasks can be rapidly added using the **QuickAdd plugin**. Simply use the QuickAdd command to create a new task with the template (`99 - Obsidian/Templates/New task.md`), which will prompt you for:

- Category
- Status
- Priority
- Difficulty
- Due date

![QuickAddTask](99%20-%20Obsidian/Attachments/README/QuickAddTask.png)
![Categories](99%20-%20Obsidian/Attachments/README/Categories.png)
![Statuses](99%20-%20Obsidian/Attachments/README/Statuses.png)
![Priorities](99%20-%20Obsidian/Attachments/README/Priorities.png)
![Difficulties](99%20-%20Obsidian/Attachments/README/Difficulties.png)

Tasks are automatically organized in `02 - Areas/Planning/Tasks/` folder.

## 📂 Vault Structure

```
├── 00 - Inbox/              # Quick capture and unprocessed notes
├── 01 - Projects/           # Active project folders
├── 02 - Areas/              # Ongoing areas of responsibility
│   ├── Planning/
│   │   ├── Daily plans/     # Organized by year/month
│   │   ├── Tasks/           # Individual task files
│   │   └── Task Manager.base
│   └── Tracking/
│       ├── Anime/           # Anime tracking database
│       └── Movies/          # Movie tracking database
├── 03 - Resources/          # Reference materials
├── 04 - Archive/            # Completed/inactive content
└── 99 - Obsidian/           # Vault configuration
    ├── Attachments/
    ├── Scripts/
    └── Templates/
```

## 🎬 Media Tracking

### Movie Database

![Movies](99%20-%20Obsidian/Attachments/README/Movies.png)

Interactive movie tracking with DataviewJS:

- Sortable columns (Title, Rating, Status, Date Watched, Rewatch Count)
- Status filtering (Watched, Planned, Dropped)
- Search functionality
- Visual posters
- Statistics dashboard

### Anime Database

![Anime](99%20-%20Obsidian/Attachments/README/Anime.png)

Similar features to Movie Database with:

- Type classification (Series/Movie)
- Cover art display
- Comprehensive filtering and sorting

Both databases use custom DataviewJS scripts for rich, interactive experiences.

## 🔧 Required Plugins

### Core Plugins

- Daily Notes
- Templates
- Canvas
- Bases
- Properties (disabled by default)

### Community Plugins

- **Homepage**: Set custom homepage
- **Icon Folder**: Add icons to folders
- **Calendar**: Visual calendar navigation
- **Style Settings**: Theme customization
- **Dataview**: Query and display data
- **Templater**: Advanced template functionality
- **Tasks**: Task management with dates
- **Table Editor**: Enhanced table editing
- **Callout Manager**: Custom callout styles
- **Tracker**: Habit tracking
- **Kanban**: Board view for tasks
- **QuickAdd**: Rapid content creation
- **Banners**: Add visual banners to notes

## 🎨 Appearance

- **Theme**: AnuPpuccin (Dark mode)
- **Accent Color**: #5ac700 (Lime green)
- **Base Font Size**: 16px
- **Custom CSS Snippets**:
    - Outlined callouts
    - Multi-column layout support
    - Extended color schemes
    - Custom rainbow colors
    - Vault logo display
    - Banner spacing for editing

## ⚙️ Configuration Highlights

- **Vim Mode**: Enabled for power users
- **Live Preview**: Disabled (uses source mode)
- **Default View**: Preview mode
- **New Notes Location**: 00 - Inbox
- **Attachments Folder**: 99 - Obsidian/Attachments
- **Template Folder**: 99 - Obsidian/Templates
- **Daily Notes Format**: `YYYY/MM - MMMM/DD.MM.YYYY (dddd)`

## 🤝 Contributing

Feel free to customize this template to fit your needs. The modular structure makes it easy to add, remove, or modify components.

## 📄 License

This template is provided as-is for personal use. Customize and adapt it as needed for your workflow.

---

**Happy note-taking! 📚✨**