```dataviewjs
const anime = dv.pages('"02 - Areas/Tracking/Anime/Anime"');
const container = this.container;

let currentSort = { column: "title", direction: 'asc' };
let activeStatus = "";

// Filter wrapper container
const filterWrapper = document.createElement("div");
filterWrapper.style.marginBottom = "1rem";
filterWrapper.style.display = "flex";
filterWrapper.style.gap = "10px";
filterWrapper.style.alignItems = "center";

// Status dropdown filter
const statusDropdown = document.createElement("div");
statusDropdown.style.position = "relative";
statusDropdown.style.display = "inline-block";

const statusButton = document.createElement("button");
statusButton.textContent = "All Statuses";
Object.assign(statusButton.style, {
    padding: "16px 20px",
    fontSize: "16px",
    border: "2px solid #ff6b6b",
    borderRadius: "25px",
    backgroundColor: "transparent",
    color: "#ff6b6b",
    cursor: "pointer",
    outline: "none",
    minWidth: "140px"
});

const statusMenu = document.createElement("div");
Object.assign(statusMenu.style, {
    position: "absolute",
    top: "100%",
    left: "0",
    backgroundColor: "rgba(255, 107, 107, 0.1)",
    border: "2px solid #ff6b6b",
    borderRadius: "10px",
    display: "none",
    zIndex: "1000",
    minWidth: "140px",
    boxShadow: "0 4px 6px rgba(0,0,0,0.1)"
});

const statusMenuOptions = [
    { value: "", text: "All Statuses" },
    { value: "watched", text: "Watched" },
    { value: "watching", text: "Watching" },
    { value: "planned", text: "Planned" },
    { value: "dropped", text: "Dropped" }
];

statusMenuOptions.forEach(option => {
    const menuItem = document.createElement("div");
    menuItem.textContent = option.text;
    Object.assign(menuItem.style, {
        padding: "12px 20px",
        cursor: "pointer",
        color: "#ff6b6b",
        borderBottom: "1px solid #ff6b6b",
        backgroundColor: "transparent"
    });
    
    menuItem.addEventListener("mouseenter", () => {
        menuItem.style.backgroundColor = "rgba(255, 107, 107, 0.6)";
        menuItem.style.color = "white";
    });
    
    menuItem.addEventListener("mouseleave", () => {
        menuItem.style.backgroundColor = "transparent";
        menuItem.style.color = "#ff6b6b";
    });
    
    menuItem.addEventListener("click", () => {
        activeStatus = option.value;
        statusButton.textContent = option.text;
        statusMenu.style.display = "none";
        renderTable(searchInput.value, activeStatus);
    });
    
    statusMenu.appendChild(menuItem);
});

statusMenu.lastChild.style.borderBottom = "none";

statusButton.addEventListener("click", (e) => {
    e.stopPropagation();
    statusMenu.style.display = statusMenu.style.display === "none" ? "block" : "none";
});

document.addEventListener("click", () => {
    statusMenu.style.display = "none";
});

statusDropdown.appendChild(statusButton);
statusDropdown.appendChild(statusMenu);
filterWrapper.appendChild(statusDropdown);

// Search input
const searchInput = document.createElement("input");
searchInput.type = "text";
searchInput.placeholder = "🔍 Search anime...";
Object.assign(searchInput.style, {
    flex: "1",
    padding: "16px",
    fontSize: "16px",
    border: "2px solid #ff6b6b",
    borderRadius: "25px",
});

// Remove focus outline
searchInput.addEventListener('focus', () => {
    searchInput.style.outline = 'none';
    searchInput.style.boxShadow = 'none';
});

searchInput.addEventListener('blur', () => {
    searchInput.style.outline = 'none';
    searchInput.style.boxShadow = 'none';
});
filterWrapper.appendChild(searchInput);

container.appendChild(filterWrapper);

// Helper functions
function sortData(data, column, direction) {
    return [...data].sort((a, b) => {
        let aVal, bVal;
        
        switch(column) {
            case 'title':
                aVal = a.file.name.toLowerCase();
                bVal = b.file.name.toLowerCase();
                break;
            case 'type':
                aVal = (a.type || "").toLowerCase();
                bVal = (b.type || "").toLowerCase();
                break;
            case 'rating':
                aVal = Number(a.rating) || 0;
                bVal = Number(b.rating) || 0;
                break;
            case 'status':
                aVal = (a.status || "").toLowerCase();
                bVal = (b.status || "").toLowerCase();
                break;
            default:
                return 0;
        }
        
        if (aVal < bVal) return direction === 'asc' ? -1 : 1;
        if (aVal > bVal) return direction === 'asc' ? 1 : -1;
        return 0;
    });
}

function getStatusEmoji(status) {
    if (!status || status === 'unknown') return 'Unknown';
    return status.charAt(0).toUpperCase() + status.slice(1);
}

function renderTable(searchTerm = "", statusFilterValue = "") {
    const oldTable = container.querySelector("table");
    if (oldTable) oldTable.remove();
    
    const oldStats = container.querySelector(".stats-div");
    if (oldStats) oldStats.remove();
    
    // Table creation
    const table = document.createElement("table");
    table.className = "dataview table-view-table";
    Object.assign(table.style, {
        width: "100%",
        borderCollapse: "separate",
        borderSpacing: "0",
        overflow: "hidden",
        border: "2px solid #ff6b6b",
        borderRadius: "15px"
    });
    
    // Table header
    const header = table.insertRow();
    
    const columns = [
        { title: "Title", key: "title", sortable: true },
        { title: "Type", key: "type", sortable: true },
        { title: "Rating", key: "rating", sortable: true },
        { title: "Status", key: "status", sortable: true },
        { title: "Poster", key: "cover", sortable: false }
    ];
    
    columns.forEach((col, index) => {
        const th = document.createElement("th");
        Object.assign(th.style, {
            padding: "12px",
            textAlign: "center",
            cursor: col.sortable ? "pointer" : "default",
            userSelect: "none",
            border: "none",
            fontWeight: "bold"
        });
        
        if (index < columns.length - 1) {
            th.style.borderRight = "2px solid #ff6b6b";
        }
        th.style.borderBottom = "2px solid #ff6b6b";
        
        if (col.sortable) {
            let displayText = col.title;
            if (currentSort.column === col.key) {
                displayText += currentSort.direction === 'asc' ? ' ↑' : ' ↓';
            }
            th.textContent = displayText;
            
            th.addEventListener('click', () => {
                if (currentSort.column === col.key) {
                    currentSort.direction = currentSort.direction === 'asc' ? 'desc' : 'asc';
                } else {
                    currentSort.column = col.key;
                    currentSort.direction = 'asc';
                }
                renderTable(searchInput.value, activeStatus);
            });
        } else {
            th.textContent = col.title;
        }
        
        header.appendChild(th);
    });
    
    // Data filtering and sorting
    let filteredAnime = [...anime];
    
    if (searchTerm) {
        filteredAnime = filteredAnime.filter(a => 
            a.file.name.toLowerCase().includes(searchTerm.toLowerCase())
        );
    }
    
    if (statusFilterValue) {
        filteredAnime = filteredAnime.filter(a => a.status === statusFilterValue);
    }
    
    if (currentSort.column) {
        filteredAnime = sortData(filteredAnime, currentSort.column, currentSort.direction);
    }
    
    // Table rows
    for (let i = 0; i < filteredAnime.length; i++) {
        const a = filteredAnime[i];
        const row = table.insertRow();
        const isLastRow = i === filteredAnime.length - 1;
        
        const cellStyle = {
            padding: "10px",
            textAlign: "center",
            verticalAlign: "middle",
            border: "none"
        };
        
        // Title cell
        const cellTitle = row.insertCell();
        Object.assign(cellTitle.style, cellStyle);
        cellTitle.style.borderRight = "2px solid #ff6b6b";
        if (!isLastRow) cellTitle.style.borderBottom = "2px solid #ff6b6b";
        
        const link = document.createElement("a");
        link.href = a.file.path;
        link.textContent = a.file.name;
        link.className = "internal-link";
        Object.assign(link.style, {
            textDecoration: "none",
            color: "#0066cc"
        });
        cellTitle.appendChild(link);
        
        // Type cell
        const cellType = row.insertCell();
        Object.assign(cellType.style, cellStyle);
        cellType.style.borderRight = "2px solid #ff6b6b";
        if (!isLastRow) cellType.style.borderBottom = "2px solid #ff6b6b";
        cellType.textContent = a.type ? a.type.charAt(0).toUpperCase() + a.type.slice(1) : "—";
        
        // Rating cell
        const cellRating = row.insertCell();
        Object.assign(cellRating.style, cellStyle);
        cellRating.style.borderRight = "2px solid #ff6b6b";
        if (!isLastRow) cellRating.style.borderBottom = "2px solid #ff6b6b";
        const rating = Number(a.rating) || 0;
        cellRating.textContent = rating > 0 ? `${rating}/10` : "—";
        
        // Status cell
        const cellStatus = row.insertCell();
        Object.assign(cellStatus.style, cellStyle);
        cellStatus.style.borderRight = "2px solid #ff6b6b";
        if (!isLastRow) cellStatus.style.borderBottom = "2px solid #ff6b6b";
        cellStatus.textContent = getStatusEmoji(a.status);
        
        const statusColors = {
            watched: "#77ff52",
            dropped: "#ff2a26",
            watching: "#73c2ff",
            planned: "#ffbb73"
        };
        if (statusColors[a.status]) {
            cellStatus.style.color = statusColors[a.status];
        }
        
        // Cover cell
        const cellCover = row.insertCell();
        Object.assign(cellCover.style, cellStyle);
        if (!isLastRow) cellCover.style.borderBottom = "2px solid #ff6b6b";
        
        if (a.cover) {
            const img = document.createElement("img");
            img.src = a.cover;
            img.width = a.cover_size || 180;
            img.style.borderRadius = "5px";
            cellCover.appendChild(img);
        } else {
            cellCover.textContent = "—";
        }
    }
    
    container.appendChild(table);
    
    // Statistics display
    const statsDiv = document.createElement("div");
    statsDiv.className = "stats-div";
    Object.assign(statsDiv.style, {
        marginTop: "1rem",
        padding: "15px",
        borderRadius: "15px",
        fontSize: "16px",
        border: "2px solid #ff6b6b"
    });
    
    const total = anime.length;
    const watched = anime.filter(a => a.status === 'watched').length;
    const dropped = anime.filter(a => a.status === 'dropped').length;
    const watching = anime.filter(a => a.status === 'watching').length;
    const planned = anime.filter(a => a.status === 'planned').length;
    
    statsDiv.innerHTML = `
        <span style="margin-right: 15px;">🌸 <strong>Total:</strong> ${total}</span>
        <span style="margin-right: 15px; color: #77ff52;"><strong>Watched:</strong> ${watched}</span>
        <span style="margin-right: 15px; color: #73c2ff;"><strong>Watching:</strong> ${watching}</span>
        <span style="margin-right: 15px; color: #ffbb73;"><strong>Planned:</strong> ${planned}</span>
        <span style="color: #ff2a26;"><strong>Dropped:</strong> ${dropped}</span>
    `;
    
    container.appendChild(statsDiv);
}

// Event listeners
searchInput.addEventListener('input', () => renderTable(searchInput.value, activeStatus));
renderTable();
```