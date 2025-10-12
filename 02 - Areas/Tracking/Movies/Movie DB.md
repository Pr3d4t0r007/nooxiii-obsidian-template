```dataviewjs
// ============================================
// INITIALIZATION AND CONSTANTS
// ============================================

const movie = dv.pages('"02 - Areas/Tracking/Movies/Movies"');
const container = this.container;

// Application state
let currentSort = { column: "title", direction: 'asc' };
let activeStatus = "";

// Style constants
const STYLES = {
    button: {
        padding: "16px 20px",
        fontSize: "16px",
        border: "2px solid #2370eb",
        borderRadius: "25px",
        backgroundColor: "transparent",
        color: "#2370eb",
        cursor: "pointer",
        outline: "none",
        minWidth: "140px"
    },
    menu: {
        position: "absolute",
        top: "100%",
        left: "0",
        backgroundColor: "rgba(35, 112, 235, 0.1)",
        border: "2px solid #2370eb",
        borderRadius: "10px",
        display: "none",
        zIndex: "1000",
        minWidth: "140px",
        boxShadow: "0 4px 6px rgba(0,0,0,0.1)"
    },
    menuItem: {
        padding: "12px 20px",
        cursor: "pointer",
        color: "#2370eb",
        borderBottom: "1px solid #2370eb",
        backgroundColor: "transparent"
    },
    cell: {
        padding: "10px",
        textAlign: "center",
        verticalAlign: "middle",
        border: "none"
    }
};

// Table column configuration
const TABLE_COLUMNS = [
    { title: "Title", key: "title", sortable: true },
    { title: "Rating", key: "rating", sortable: true },
    { title: "Status", key: "status", sortable: true },
    { title: "Thoughts", key: "thoughts", sortable: true },
    { title: "Date Watched", key: "date_watched", sortable: true },
    { title: "Rewatch Count", key: "rewatch_count", sortable: true },
    { title: "Poster", key: "poster", sortable: false }
];

// Status filter dropdown options
const STATUS_OPTIONS = [
    { value: "", text: "All Statuses" },
    { value: "watched", text: "Watched" },
    { value: "planned", text: "Planned" },
    { value: "dropped", text: "Dropped" }
];

// Status color mapping
const STATUS_COLORS = {
    watched: "#77ff52",
    dropped: "#ff2a26",
    planned: "#ffbb73"
};

// ============================================
// UTILITY FUNCTIONS
// ============================================

/**
 * Applies styles to DOM element
 * 
 * element - target DOM element, 
 * styles - style object to apply
 */
function applyStyles(element, styles) {
    Object.assign(element.style, styles);
}

/**
 * Creates DOM element with styles and attributes
 * 
 * tag - HTML tag name, 
 * styles - CSS styles object, 
 * attributes - element attributes
 */
function createElement(tag, styles = {}, attributes = {}) {
    const element = document.createElement(tag);
    applyStyles(element, styles);
    Object.entries(attributes).forEach(([key, value]) => {
        element[key] = value;
    });
    return element;
}

/**
 * Sorts movie data by specified column and direction
 * 
 * data - array of movie objects
 * column - column key to sort by
 * direction - 'asc' or 'desc'
 */
function sortData(data, column, direction) {
    return [...data].sort((m1, m2) => {
        const getValue = (movie, col) => {
            switch(col) {
                case 'title':
                    return movie.file.name.toLowerCase();
                case 'rating':
                case 'rewatch_count':
                    return Number(movie[col]) || 0;
                case 'status':
                case 'thoughts':
                    return (movie[col] || "").toLowerCase();
                case 'date_watched':
                    if (!movie.date_watched) return new Date(0);
                    const [day, month, year] = movie.date_watched.split('.');
                    return new Date(year, month - 1, day);
                default:
                    return "";
            }
        };
        
        const val1 = getValue(m1, column);
        const val2 = getValue(m2, column);
        
        if (val1 < val2) return direction === 'asc' ? -1 : 1;
        if (val1 > val2) return direction === 'asc' ? 1 : -1;
        return 0;
    });
}

/**
 * Formats movie status for display
 * 
 * status - raw status string from movie data
 */
function formatStatus(status) {
    return !status || status === 'unknown' ? 'Unknown' : 
           status.charAt(0).toUpperCase() + status.slice(1);
}

/**
 * Adds borders to table cells based on position
 * 
 * cell - table cell element, 
 * isLastColumn - is this the rightmost column, 
 * isLastRow - is this the bottom row
 */
function addCellBorders(cell, isLastColumn = false, isLastRow = false) {
    if (!isLastColumn) cell.style.borderRight = "2px solid #2370eb";
    if (!isLastRow) cell.style.borderBottom = "2px solid #2370eb";
}

// ============================================
// UI COMPONENT CREATION
// ============================================

/**
 * Creates filter wrapper container
 */
function createFilterWrapper() {
    return createElement("div", {
        marginBottom: "1rem",
        display: "flex",
        gap: "10px",
        alignItems: "center"
    });
}

/**
 * Creates status filter dropdown with all options and event handlers
 */
function createStatusDropdown() {
    const dropdown = createElement("div", { position: "relative", display: "inline-block" });
    const button = createElement("button", STYLES.button, { textContent: "All Statuses" });
    const menu = createElement("div", STYLES.menu);
    
    // Create menu items with hover effects and click handlers
    STATUS_OPTIONS.forEach(option => {
        const menuItem = createElement("div", STYLES.menuItem, { textContent: option.text });
        
        // Hover effects
        menuItem.addEventListener("mouseenter", () => {
            applyStyles(menuItem, { 
                backgroundColor: "rgba(35, 112, 235, 0.6)", 
                color: "white" 
            });
        });
        
        menuItem.addEventListener("mouseleave", () => {
            applyStyles(menuItem, { 
                backgroundColor: "transparent", 
                color: "#2370eb" 
            });
        });
        
        // Click handler
        menuItem.addEventListener("click", () => {
            activeStatus = option.value;
            button.textContent = option.text;
            menu.style.display = "none";
            renderTable(searchInput.value, activeStatus);
        });
        
        menu.appendChild(menuItem);
    });
    
    // Remove border from last menu item
    menu.lastChild.style.borderBottom = "none";
    
    // Button click handler
    button.addEventListener("click", (e) => {
        e.stopPropagation();
        menu.style.display = menu.style.display === "none" ? "block" : "none";
    });
    
    dropdown.appendChild(button);
    dropdown.appendChild(menu);
    return dropdown;
}

/**
 * Creates search input field with focus handling
 */
function createSearchInput() {
    const input = createElement("input", {
        flex: "1",
        padding: "16px",
        fontSize: "16px",
        border: "2px solid #2370eb",
        borderRadius: "25px",
    }, {
        type: "text",
        placeholder: "🔍 Search movie..."
    });
    
    // Remove outline on focus/blur
    ['focus', 'blur'].forEach(event => {
        input.addEventListener(event, () => {
            applyStyles(input, { outline: 'none', boxShadow: 'none' });
        });
    });
    
    return input;
}

// ============================================
// TABLE CREATION
// ============================================

/**
 * Creates table header with sortable columns
 * 
 * table - table element to add header to
 */
function createTableHeader(table) {
    const header = table.insertRow();
    
    TABLE_COLUMNS.forEach((col, index) => {
        const th = createElement("th", {
            padding: "12px",
            textAlign: "center",
            cursor: col.sortable ? "pointer" : "default",
            userSelect: "none",
            border: "none",
            fontWeight: "bold"
        });
        
        addCellBorders(th, index === TABLE_COLUMNS.length - 1, false);
        
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
}

/**
 * Creates table cell with movie data based on column type
 * 
 * row - table row element, 
 * movie - movie data object, 
 * columnKey - column identifier, 
 * isLastColumn - rightmost column flag, 
 * isLastRow - bottom row flag
 */
function createTableCell(row, movie, columnKey, isLastColumn, isLastRow) {
    const cell = row.insertCell();
    applyStyles(cell, STYLES.cell);
    addCellBorders(cell, isLastColumn, isLastRow);
    
    switch(columnKey) {
        case 'title':
            const link = createElement("a", {
                textDecoration: "none",
                color: "#0066cc"
            }, {
                href: movie.file.path,
                textContent: movie.file.name,
                className: "internal-link"
            });
            cell.appendChild(link);
            break;
            
        case 'rating':
            cell.textContent = Number(movie.rating) ? `${movie.rating}/10` : "?/10";
            break;
            
        case 'status':
            cell.textContent = formatStatus(movie.status);
            if (STATUS_COLORS[movie.status]) {
                cell.style.color = STATUS_COLORS[movie.status];
            }
            break;
            
        case 'rewatch_count':
            cell.textContent = movie.rewatch_count;
            break;
            
        case 'poster':
            if (movie.poster) {
                const img = createElement("img", { borderRadius: "5px" }, {
                    src: movie.poster,
                    width: movie.poster_size || 180
                });
                cell.appendChild(img);
            } else {
                cell.textContent = "—";
            }
            break;
            
        default:
            cell.textContent = movie[columnKey] || "—";
    }
}

/**
 * Creates statistics display block with movie counts by status
 */
function createStatsDiv() {
    const statsDiv = createElement("div", {
        marginTop: "1rem",
        padding: "15px",
        borderRadius: "15px",
        fontSize: "16px",
        border: "2px solid #2370eb"
    }, {
        className: "stats-div"
    });
    
    const total = movie.length;
    const watched = movie.filter(m => m.status === 'watched').length;
    const dropped = movie.filter(m => m.status === 'dropped').length;
    const planned = movie.filter(m => m.status === 'planned').length;
    
    statsDiv.innerHTML = `
        <span style="margin-right: 15px;"><strong>Total:</strong> ${total}</span>
        <span style="margin-right: 15px; color: ${STATUS_COLORS.watched};"><strong>Watched:</strong> ${watched}</span>
        <span style="margin-right: 15px; color: ${STATUS_COLORS.planned};"><strong>Planned:</strong> ${planned}</span>
        <span style="color: ${STATUS_COLORS.dropped};"><strong>Dropped:</strong> ${dropped}</span>
    `;
    
    return statsDiv;
}

// ============================================
// MAIN RENDERING LOGIC
// ============================================

/**
 * Main table rendering function with filtering and sorting
 * 
 * searchTerm - text to filter movie titles, 
 * statusFilterValue - status filter selection
 */
function renderTable(searchTerm = "", statusFilterValue = "") {
    // Remove existing table and stats elements
    [".dataview.table-view-table", ".stats-div"].forEach(selector => {
        const element = container.querySelector(selector);
        if (element) element.remove();
    });
    
    // Create new table with styling
    const table = createElement("table", {
        width: "100%",
        borderCollapse: "separate",
        borderSpacing: "0",
        overflow: "hidden",
        border: "2px solid #2370eb",
        borderRadius: "15px"
    }, {
        className: "dataview table-view-table"
    });
    
    createTableHeader(table);
    
    // Filter and sort data
    let filteredMovie = [...movie];
    
    if (searchTerm) {
        filteredMovie = filteredMovie.filter(m => 
            m.file.name.toLowerCase().includes(searchTerm.toLowerCase())
        );
    }
    
    if (statusFilterValue) {
        filteredMovie = filteredMovie.filter(m => m.status === statusFilterValue);
    }
    
    if (currentSort.column) {
        filteredMovie = sortData(filteredMovie, currentSort.column, currentSort.direction);
    }
    
    // Create table rows
    filteredMovie.forEach((movie, rowIndex) => {
        const row = table.insertRow();
        const isLastRow = rowIndex === filteredMovie.length - 1;
        
        TABLE_COLUMNS.forEach((column, colIndex) => {
            const isLastColumn = colIndex === TABLE_COLUMNS.length - 1;
            createTableCell(row, movie, column.key, isLastColumn, isLastRow);
        });
    });
    
    // Add elements to DOM
    container.appendChild(table);
    container.appendChild(createStatsDiv());
}

// ============================================
// INITIALIZATION
// ============================================

// Create UI components
const filterWrapper = createFilterWrapper();
const statusDropdown = createStatusDropdown();
const searchInput = createSearchInput();

filterWrapper.appendChild(statusDropdown);
filterWrapper.appendChild(searchInput);
container.appendChild(filterWrapper);

// Close dropdown when clicking outside
document.addEventListener("click", () => {
    const menu = statusDropdown.querySelector("div[style*='position: absolute']");
    if (menu) menu.style.display = "none";
});

// Search input event handler
searchInput.addEventListener('input', () => renderTable(searchInput.value, activeStatus));

// Initial render
renderTable();
```