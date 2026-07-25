# Frontend Test Run Implementation - Consumer Credit Rating Data Scraper

A comprehensive, standalone frontend test implementation for validating the consumer credit rating data scraping agent without external dependencies.

---

## Project Structure

```
test-run-frontend/
├── index.html
├── styles.css
├── app.js
├── test-data.js
├── utils.js
├── README.md
└── assets/
    └── sample-data/
        ├── sample_consumers.json
        ├── sample_ratings.json
        └── sample_results.json
```

---

## 1. HTML Interface (index.html)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Consumer Credit Rating Data Scraper - Test Run</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <!-- Header -->
        <header class="header">
            <div class="header-content">
                <h1>🔍 Consumer Credit Rating Data Scraper</h1>
                <p class="subtitle">Test Run Implementation - Standalone Frontend</p>
            </div>
            <div class="status-badge" id="statusBadge">Ready</div>
        </header>

        <!-- Main Navigation -->
        <nav class="nav-tabs">
            <button class="nav-btn active" data-tab="dashboard">Dashboard</button>
            <button class="nav-btn" data-tab="data-collection">Data Collection</button>
            <button class="nav-btn" data-tab="data-mapping">Data Mapping</button>
            <button class="nav-btn" data-tab="categorization">Categorization</button>
            <button class="nav-btn" data-tab="results">Results</button>
            <button class="nav-btn" data-tab="logs">Logs</button>
        </nav>

        <!-- Content Tabs -->
        <div class="tab-content">

            <!-- Dashboard Tab -->
            <div id="dashboard" class="tab-pane active">
                <div class="dashboard-grid">
                    <!-- Executive Summary -->
                    <section class="dashboard-card">
                        <h2>Executive Summary</h2>
                        <div class="summary-stats">
                            <div class="stat-box">
                                <span class="stat-label">Total Consumers</span>
                                <span class="stat-value" id="totalConsumers">0</span>
                            </div>
                            <div class="stat-box">
                                <span class="stat-label">Mapped to Ratings</span>
                                <span class="stat-value" id="mappedConsumers">0</span>
                            </div>
                            <div class="stat-box">
                                <span class="stat-label">Mapping Success %</span>
                                <span class="stat-value" id="mappingSuccess">0%</span>
                            </div>
                            <div class="stat-box">
                                <span class="stat-label">Processing Time</span>
                                <span class="stat-value" id="processingTime">0s</span>
                            </div>
                        </div>
                    </section>

                    <!-- Rating Distribution -->
                    <section class="dashboard-card">
                        <h2>Rating Distribution</h2>
                        <div id="ratingChart" class="chart-placeholder"></div>
                        <div class="chart-legend">
                            <div class="legend-item"><span class="legend-color investment"></span>Investment Grade</div>
                            <div class="legend-item"><span class="legend-color speculative"></span>Speculative Grade</div>
                            <div class="legend-item"><span class="legend-color below-investment"></span>Below Investment</div>
                            <div class="legend-item"><span class="legend-color unrated"></span>Unrated</div>
                        </div>
                    </section>

                    <!-- State-wise Distribution -->
                    <section class="dashboard-card">
                        <h2>State-wise Distribution</h2>
                        <div class="state-distribution">
                            <div class="state-item">
                                <span class="state-name">Haryana</span>
                                <div class="state-bar">
                                    <div class="state-progress" style="width: 0%" id="haryana-bar"></div>
                                </div>
                                <span class="state-count" id="haryana-count">0</span>
                            </div>
                            <div class="state-item">
                                <span class="state-name">Uttar Pradesh</span>
                                <div class="state-bar">
                                    <div class="state-progress" style="width: 0%" id="up-bar"></div>
                                </div>
                                <span class="state-count" id="up-count">0</span>
                            </div>
                            <div class="state-item">
                                <span class="state-name">Karnataka</span>
                                <div class="state-bar">
                                    <div class="state-progress" style="width: 0%" id="karnataka-bar"></div>
                                </div>
                                <span class="state-count" id="karnataka-count">0</span>
                            </div>
                            <div class="state-item">
                                <span class="state-name">Uttarakhand</span>
                                <div class="state-bar">
                                    <div class="state-progress" style="width: 0%" id="uttarakhand-bar"></div>
                                </div>
                                <span class="state-count" id="uttarakhand-count">0</span>
                            </div>
                        </div>
                    </section>

                    <!-- Load Distribution -->
                    <section class="dashboard-card">
                        <h2>Load Tier Distribution</h2>
                        <div class="load-distribution">
                            <div class="load-tier">
                                <span class="tier-name">>50 MWp</span>
                                <span class="tier-count" id="tier1-count">0</span>
                            </div>
                            <div class="load-tier">
                                <span class="tier-name">25-50 MWp</span>
                                <span class="tier-count" id="tier2-count">0</span>
                            </div>
                            <div class="load-tier">
                                <span class="tier-name">10-25 MWp</span>
                                <span class="tier-count" id="tier3-count">0</span>
                            </div>
                        </div>
                    </section>
                </div>
            </div>

            <!-- Data Collection Tab -->
            <div id="data-collection" class="tab-pane">
                <div class="section-container">
                    <h2>Phase 1: Data Collection</h2>
                    
                    <div class="control-panel">
                        <div class="control-group">
                            <label for="consumerCount">Number of Test Consumers:</label>
                            <input type="number" id="consumerCount" value="50" min="10" max="500">
                        </div>
                        <div class="control-group">
                            <label for="ratingCount">Number of Test Ratings:</label>
                            <input type="number" id="ratingCount" value="30" min="5" max="200">
                        </div>
                        <button class="btn btn-primary" onclick="runDataCollection()">
                            ▶ Start Data Collection
                        </button>
                    </div>

                    <!-- DISCOM Data Collection -->
                    <section class="data-section">
                        <h3>DISCOM Data Extraction</h3>
                        <div class="progress-container">
                            <div class="progress-bar">
                                <div class="progress-fill" id="discom-progress" style="width: 0%"></div>
                            </div>
                            <span class="progress-text" id="discom-text">Ready</span>
                        </div>
                        <div class="data-preview" id="discom-preview"></div>
                    </section>

                    <!-- Transco Data Collection -->
                    <section class="data-section">
                        <h3>Transco Data Extraction</h3>
                        <div class="progress-container">
                            <div class="progress-bar">
                                <div class="progress-fill" id="transco-progress" style="width: 0%"></div>
                            </div>
                            <span class="progress-text" id="transco-text">Ready</span>
                        </div>
                        <div class="data-preview" id="transco-preview"></div>
                    </section>

                    <!-- Nodal Agency Data Collection -->
                    <section class="data-section">
                        <h3>State Nodal Agency Data Extraction</h3>
                        <div class="progress-container">
                            <div class="progress-bar">
                                <div class="progress-fill" id="nodal-progress" style="width: 0%"></div>
                            </div>
                            <span class="progress-text" id="nodal-text">Ready</span>
                        </div>
                        <div class="data-preview" id="nodal-preview"></div>
                    </section>

                    <!-- Rating Agency Data Collection -->
                    <section class="data-section">
                        <h3>Rating Agency Data Scraping</h3>
                        <div class="rating-sources">
                            <div class="rating-source">
                                <h4>CRISIL</h4>
                                <div class="progress-container">
                                    <div class="progress-bar">
                                        <div class="progress-fill" id="crisil-progress" style="width: 0%"></div>
                                    </div>
                                    <span class="progress-text" id="crisil-text">Ready</span>
                                </div>
                            </div>
                            <div class="rating-source">
                                <h4>ICRA</h4>
                                <div class="progress-container">
                                    <div class="progress-bar">
                                        <div class="progress-fill" id="icra-progress" style="width: 0%"></div>
                                    </div>
                                    <span class="progress-text" id="icra-text">Ready</span>
                                </div>
                            </div>
                            <div class="rating-source">
                                <h4>CARE EDGE</h4>
                                <div class="progress-container">
                                    <div class="progress-bar">
                                        <div class="progress-fill" id="care-progress" style="width: 0%"></div>
                                    </div>
                                    <span class="progress-text" id="care-text">Ready</span>
                                </div>
                            </div>
                            <div class="rating-source">
                                <h4>S&P Global</h4>
                                <div class="progress-container">
                                    <div class="progress-bar">
                                        <div class="progress-fill" id="sp-progress" style="width: 0%"></div>
                                    </div>
                                    <span class="progress-text" id="sp-text">Ready</span>
                                </div>
                            </div>
                        </div>
                    </section>
                </div>
            </div>

            <!-- Data Mapping Tab -->
            <div id="data-mapping" class="tab-pane">
                <div class="section-container">
                    <h2>Phase 2: Data Mapping & Cross-Linking</h2>

                    <div class="control-panel">
                        <button class="btn btn-primary" onclick="runDataMapping()">
                            ▶ Start Data Mapping
                        </button>
                    </div>

                    <!-- Normalization -->
                    <section class="data-section">
                        <h3>Step 1: Data Normalization</h3>
                        <div class="progress-container">
                            <div class="progress-bar">
                                <div class="progress-fill" id="normalize-progress" style="width: 0%"></div>
                            </div>
                            <span class="progress-text" id="normalize-text">Ready</span>
                        </div>
                        <div class="details-box" id="normalize-details"></div>
                    </section>

                    <!-- Fuzzy Matching -->
                    <section class="data-section">
                        <h3>Step 2: Fuzzy String Matching</h3>
                        <div class="progress-container">
                            <div class="progress-bar">
                                <div class="progress-fill" id="matching-progress" style="width: 0%"></div>
                            </div>
                            <span class="progress-text" id="matching-text">Ready</span>
                        </div>
                        <div class="matching-stats" id="matching-stats"></div>
                    </section>

                    <!-- Cross-Validation -->
                    <section class="data-section">
                        <h3>Step 3: Cross-Validation & De-duplication</h3>
                        <div class="progress-container">
                            <div class="progress-bar">
                                <div class="progress-fill" id="validation-progress" style="width: 0%"></div>
                            </div>
                            <span class="progress-text" id="validation-text">Ready</span>
                        </div>
                        <div class="validation-summary" id="validation-summary"></div>
                    </section>
                </div>
            </div>

            <!-- Categorization Tab -->
            <div id="categorization" class="tab-pane">
                <div class="section-container">
                    <h2>Phase 3: Data Categorization</h2>

                    <div class="control-panel">
                        <button class="btn btn-primary" onclick="runCategorization()">
                            ▶ Start Categorization
                        </button>
                    </div>

                    <!-- Rating Category Breakdown -->
                    <section class="data-section">
                        <h3>Rating-Based Categorization</h3>
                        <div class="progress-container">
                            <div class="progress-bar">
                                <div class="progress-fill" id="category-progress" style="width: 0%"></div>
                            </div>
                            <span class="progress-text" id="category-text">Ready</span>
                        </div>
                        <div class="category-breakdown" id="category-breakdown"></div>
                    </section>

                    <!-- Outlook Categorization -->
                    <section class="data-section">
                        <h3>Outlook-Based Categorization</h3>
                        <div class="outlook-breakdown" id="outlook-breakdown"></div>
                    </section>

                    <!-- Sector Categorization -->
                    <section class="data-section">
                        <h3>Sector-Based Categorization</h3>
                        <div class="sector-breakdown" id="sector-breakdown"></div>
                    </section>

                    <!-- Load Tier Categorization -->
                    <section class="data-section">
                        <h3>Load Tier Distribution</h3>
                        <div class="load-tier-breakdown" id="load-tier-breakdown"></div>
                    </section>
                </div>
            </div>

            <!-- Results Tab -->
            <div id="results" class="tab-pane">
                <div class="section-container">
                    <h2>Test Results & Outputs</h2>

                    <div class="control-panel">
                        <button class="btn btn-secondary" onclick="exportResults('json')">
                            📥 Export as JSON
                        </button>
                        <button class="btn btn-secondary" onclick="exportResults('csv')">
                            📥 Export as CSV
                        </button>
                        <button class="btn btn-secondary" onclick="clearAll()">
                            🔄 Clear All
                        </button>
                    </div>

                    <!-- Master Consumer Registry -->
                    <section class="data-section">
                        <h3>Master Consumer Registry</h3>
                        <div class="table-container" id="consumer-registry"></div>
                    </section>

                    <!-- Credit Rating Master -->
                    <section class="data-section">
                        <h3>Credit Rating Master Table</h3>
                        <div class="table-container" id="rating-master"></div>
                    </section>

                    <!-- Consumer-Rating Linkage -->
                    <section class="data-section">
                        <h3>Consumer-Rating Linkage</h3>
                        <div class="table-container" id="linkage-table"></div>
                    </section>

                    <!-- Summary Statistics -->
                    <section class="data-section">
                        <h3>Summary Statistics</h3>
                        <div class="stats-summary" id="stats-summary"></div>
                    </section>
                </div>
            </div>

            <!-- Logs Tab -->
            <div id="logs" class="tab-pane">
                <div class="section-container">
                    <h2>Processing Logs</h2>

                    <div class="control-panel">
                        <button class="btn btn-secondary" onclick="clearLogs()">
                            🗑️ Clear Logs
                        </button>
                        <select id="logLevel" onchange="filterLogs()">
                            <option value="all">All Levels</option>
                            <option value="info">Info</option>
                            <option value="success">Success</option>
                            <option value="warning">Warning</option>
                            <option value="error">Error</option>
                        </select>
                    </div>

                    <div class="logs-container" id="logsContainer"></div>
                </div>
            </div>
        </div>
    </div>

    <!-- Footer -->
    <footer class="footer">
        <p>Consumer Credit Rating Data Scraper - Test Run v1.0 | Standalone Frontend Implementation</p>
    </footer>

    <script src="utils.js"></script>
    <script src="test-data.js"></script>
    <script src="app.js"></script>
</body>
</html>
```

---

## 2. CSS Styling (styles.css)

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

:root {
    --primary-color: #2563eb;
    --secondary-color: #64748b;
    --success-color: #16a34a;
    --warning-color: #ea580c;
    --danger-color: #dc2626;
    --info-color: #0891b2;
    
    --investment-grade: #059669;
    --speculative-grade: #f59e0b;
    --below-investment: #ef4444;
    --unrated: #9ca3af;
    
    --bg-primary: #ffffff;
    --bg-secondary: #f8fafc;
    --bg-tertiary: #f1f5f9;
    --text-primary: #1e293b;
    --text-secondary: #64748b;
    --border-color: #e2e8f0;
    
    --shadow: 0 1px 3px rgba(0, 0, 0, 0.1), 0 1px 2px rgba(0, 0, 0, 0.06);
    --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
}

body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
    background-color: var(--bg-secondary);
    color: var(--text-primary);
    line-height: 1.6;
}

.container {
    max-width: 1400px;
    margin: 0 auto;
    background-color: var(--bg-primary);
}

/* Header */
.header {
    background: linear-gradient(135deg, var(--primary-color), var(--info-color));
    color: white;
    padding: 40px 20px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    box-shadow: var(--shadow-lg);
}

.header-content h1 {
    font-size: 32px;
    margin-bottom: 8px;
}

.header-content .subtitle {
    font-size: 14px;
    opacity: 0.9;
}

.status-badge {
    background-color: rgba(255, 255, 255, 0.3);
    color: white;
    padding: 8px 16px;
    border-radius: 20px;
    font-weight: 600;
    font-size: 14px;
}

.status-badge.processing {
    background-color: var(--warning-color);
}

.status-badge.success {
    background-color: var(--success-color);
}

/* Navigation Tabs */
.nav-tabs {
    display: flex;
    background-color: var(--bg-secondary);
    border-bottom: 2px solid var(--border-color);
    overflow-x: auto;
    padding: 0 20px;
}

.nav-btn {
    background: none;
    border: none;
    padding: 16px 20px;
    cursor: pointer;
    font-size: 14px;
    font-weight: 500;
    color: var(--text-secondary);
    border-bottom: 3px solid transparent;
    transition: all 0.3s ease;
    white-space: nowrap;
}

.nav-btn:hover {
    color: var(--primary-color);
}

.nav-btn.active {
    color: var(--primary-color);
    border-bottom-color: var(--primary-color);
}

/* Tab Content */
.tab-content {
    padding: 40px 20px;
    min-height: 600px;
}

.tab-pane {
    display: none;
}

.tab-pane.active {
    display: block;
    animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
}

/* Dashboard */
.dashboard-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 24px;
    margin-bottom: 40px;
}

.dashboard-card {
    background-color: var(--bg-primary);
    border: 1px solid var(--border-color);
    border-radius: 8px;
    padding: 24px;
    box-shadow: var(--shadow);
}

.dashboard-card h2 {
    font-size: 18px;
    margin-bottom: 20px;
    color: var(--text-primary);
}

.summary-stats {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 16px;
}

.stat-box {
    display: flex;
    flex-direction: column;
    background-color: var(--bg-secondary);
    padding: 16px;
    border-radius: 6px;
    text-align: center;
}

.stat-label {
    font-size: 12px;
    color: var(--text-secondary);
    margin-bottom: 8px;
    text-transform: uppercase;
    letter-spacing: 0.5px;
}

.stat-value {
    font-size: 24px;
    font-weight: bold;
    color: var(--primary-color);
}

/* State Distribution */
.state-distribution {
    display: flex;
    flex-direction: column;
    gap: 16px;
}

.state-item {
    display: grid;
    grid-template-columns: 100px 1fr 60px;
    align-items: center;
    gap: 12px;
}

.state-name {
    font-weight: 500;
    font-size: 14px;
}

.state-bar {
    background-color: var(--bg-secondary);
    border-radius: 4px;
    height: 24px;
    overflow: hidden;
}

.state-progress {
    background: linear-gradient(90deg, var(--primary-color), var(--info-color));
    height: 100%;
    border-radius: 4px;
    transition: width 0.3s ease;
}

.state-count {
    text-align: right;
    font-weight: 600;
    font-size: 14px;
    color: var(--text-primary);
}

/* Load Distribution */
.load-distribution {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;
}

.load-tier {
    background-color: var(--bg-secondary);
    padding: 16px;
    border-radius: 6px;
    text-align: center;
}

.tier-name {
    display: block;
    font-size: 12px;
    color: var(--text-secondary);
    margin-bottom: 8px;
}

.tier-count {
    display: block;
    font-size: 24px;
    font-weight: bold;
    color: var(--primary-color);
}

/* Section Container */
.section-container {
    max-width: 100%;
}

.section-container h2 {
    font-size: 24px;
    margin-bottom: 24px;
    color: var(--text-primary);
}

/* Control Panel */
.control-panel {
    background-color: var(--bg-secondary);
    padding: 20px;
    border-radius: 8px;
    margin-bottom: 24px;
    display: flex;
    gap: 16px;
    flex-wrap: wrap;
    align-items: center;
}

.control-group {
    display: flex;
    gap: 8px;
    align-items: center;
}

.control-group label {
    font-size: 14px;
    font-weight: 500;
    color: var(--text-secondary);
}

.control-group input {
    padding: 8px 12px;
    border: 1px solid var(--border-color);
    border-radius: 4px;
    font-size: 14px;
    width: 100px;
}

.control-group select {
    padding: 8px 12px;
    border: 1px solid var(--border-color);
    border-radius: 4px;
    font-size: 14px;
}

/* Buttons */
.btn {
    padding: 10px 20px;
    border: none;
    border-radius: 6px;
    font-size: 14px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s ease;
    display: inline-flex;
    align-items: center;
    gap: 8px;
}

.btn-primary {
    background-color: var(--primary-color);
    color: white;
}

.btn-primary:hover {
    background-color: #1d4ed8;
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(37, 99, 235, 0.3);
}

.btn-secondary {
    background-color: var(--secondary-color);
    color: white;
}

.btn-secondary:hover {
    background-color: #475569;
}

.btn:disabled {
    opacity: 0.5;
    cursor: not-allowed;
}

/* Data Section */
.data-section {
    margin-bottom: 32px;
}

.data-section h3 {
    font-size: 18px;
    margin-bottom: 16px;
    color: var(--text-primary);
    display: flex;
    align-items: center;
    gap: 8px;
}

/* Progress Bar */
.progress-container {
    margin-bottom: 12px;
}

.progress-bar {
    background-color: var(--bg-secondary);
    border-radius: 8px;
    height: 24px;
    overflow: hidden;
    margin-bottom: 8px;
}

.progress-fill {
    background: linear-gradient(90deg, var(--primary-color), var(--info-color));
    height: 100%;
    border-radius: 8px;
    transition: width 0.3s ease;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 12px;
    color: white;
    font-weight: 600;
}

.progress-text {
    font-size: 12px;
    color: var(--text-secondary);
}

/* Data Preview */
.data-preview {
    background-color: var(--bg-secondary);
    border-left: 4px solid var(--primary-color);
    padding: 12px;
    border-radius: 4px;
    font-size: 12px;
    font-family: 'Courier New', monospace;
    color: var(--text-secondary);
    max-height: 150px;
    overflow-y: auto;
}

/* Rating Sources Grid */
.rating-sources {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 16px;
}

.rating-source {
    background-color: var(--bg-secondary);
    padding: 16px;
    border-radius: 6px;
}

.rating-source h4 {
    font-size: 14px;
    margin-bottom: 12px;
    color: var(--text-primary);
}

/* Matching Stats */
.matching-stats {
    background-color: var(--bg-secondary);
    padding: 16px;
    border-radius: 6px;
}

.matching-stat {
    display: flex;
    justify-content: space-between;
    padding: 8px 0;
    border-bottom: 1px solid var(--border-color);
    font-size: 14px;
}

.matching-stat:last-child {
    border-bottom: none;
}

/* Category Breakdown */
.category-breakdown {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 16px;
}

.category-item {
    background-color: var(--bg-secondary);
    padding: 16px;
    border-radius: 6px;
    border-left: 4px solid var(--primary-color);
}

.category-item.investment-grade {
    border-left-color: var(--investment-grade);
}

.category-item.speculative-grade {
    border-left-color: var(--speculative-grade);
}

.category-item.below-investment-grade {
    border-left-color: var(--below-investment);
}

.category-item.unrated {
    border-left-color: var(--unrated);
}

.category-name {
    font-weight: 600;
    font-size: 14px;
    margin-bottom: 8px;
    color: var(--text-primary);
}

.category-stats {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 8px;
    font-size: 12px;
}

.category-stat-label {
    color: var(--text-secondary);
}

.category-stat-value {
    font-weight: 600;
    color: var(--text-primary);
}

/* Table Container */
.table-container {
    overflow-x: auto;
    border: 1px solid var(--border-color);
    border-radius: 6px;
}

table {
    width: 100%;
    border-collapse: collapse;
    font-size: 13px;
}

thead {
    background-color: var(--bg-secondary);
    border-bottom: 2px solid var(--border-color);
}

th {
    padding: 12px;
    text-align: left;
    font-weight: 600;
    color: var(--text-primary);
}

td {
    padding: 12px;
    border-bottom: 1px solid var(--border-color);
}

tbody tr:hover {
    background-color: var(--bg-secondary);
}

/* Logs Container */
.logs-container {
    background-color: var(--bg-secondary);
    border: 1px solid var(--border-color);
    border-radius: 6px;
    padding: 0;
    font-family: 'Courier New', monospace;
    font-size: 12px;
    max-height: 600px;
    overflow-y: auto;
}

.log-entry {
    padding: 10px 16px;
    border-bottom: 1px solid var(--border-color);
    display: flex;
    gap: 12px;
    align-items: flex-start;
}

.log-entry:last-child {
    border-bottom: none;
}

.log-timestamp {
    color: var(--text-secondary);
    min-width: 120px;
}

.log-level {
    font-weight: 600;
    min-width: 70px;
    text-transform: uppercase;
    font-size: 11px;
}

.log-level.info {
    color: var(--info-color);
}

.log-level.success {
    color: var(--success-color);
}

.log-level.warning {
    color: var(--warning-color);
}

.log-level.error {
    color: var(--danger-color);
}

.log-message {
    flex: 1;
    color: var(--text-primary);
    word-break: break-word;
}

/* Footer */
.footer {
    background-color: var(--bg-secondary);
    border-top: 1px solid var(--border-color);
    padding: 20px;
    text-align: center;
    color: var(--text-secondary);
    font-size: 12px;
}

/* Chart Legend */
.chart-legend {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 12px;
}

.legend-item {
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 13px;
}

.legend-color {
    width: 16px;
    height: 16px;
    border-radius: 3px;
}

.legend-color.investment {
    background-color: var(--investment-grade);
}

.legend-color.speculative {
    background-color: var(--speculative-grade);
}

.legend-color.below-investment {
    background-color: var(--below-investment);
}

.legend-color.unrated {
    background-color: var(--unrated);
}

/* Chart Placeholder */
.chart-placeholder {
    background-color: var(--bg-secondary);
    border: 1px dashed var(--border-color);
    border-radius: 6px;
    height: 250px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--text-secondary);
    margin-bottom: 16px;
}

/* Responsive */
@media (max-width: 768px) {
    .header {
        flex-direction: column;
        gap: 16px;
        text-align: center;
    }

    .dashboard-grid {
        grid-template-columns: 1fr;
    }

    .summary-stats {
        grid-template-columns: 1fr;
    }

    .control-panel {
        flex-direction: column;
        align-items: flex-start;
    }

    .rating-sources {
        grid-template-columns: 1fr;
    }

    .load-distribution {
        grid-template-columns: 1fr;
    }

    table {
        font-size: 12px;
    }

    th, td {
        padding: 8px;
    }
}
```

---

## 3. JavaScript Main Application (app.js)

```javascript
// Global State
let appState = {
    consumers: [],
    ratings: [],
    linkages: [],
    results: {},
    logs: [],
    startTime: null
};

// Initialize App
document.addEventListener('DOMContentLoaded', function() {
    initializeApp();
    setupEventListeners();
});

function initializeApp() {
    addLog('info', 'Application initialized successfully');
    updateDashboard();
}

function setupEventListeners() {
    // Tab Navigation
    document.querySelectorAll('.nav-btn').forEach(btn => {
        btn.addEventListener('click', function() {
            const tabName = this.getAttribute('data-tab');
            switchTab(tabName);
        });
    });
}

// Tab Switching
function switchTab(tabName) {
    // Hide all tabs
    document.querySelectorAll('.tab-pane').forEach(pane => {
        pane.classList.remove('active');
    });

    // Remove active class from nav buttons
    document.querySelectorAll('.nav-btn').forEach(btn => {
        btn.classList.remove('active');
    });

    // Show selected tab
    document.getElementById(tabName).classList.add('active');
    document.querySelector(`[data-tab="${tabName}"]`).classList.add('active');

    addLog('info', `Switched to ${tabName} tab`);
}

// Data Collection Phase
function runDataCollection() {
    setStatus('processing');
    appState.startTime = Date.now();
    const consumerCount = parseInt(document.getElementById('consumerCount').value);
    const ratingCount = parseInt(document.getElementById('ratingCount').value);

    addLog('info', `Starting data collection for ${consumerCount} consumers and ${ratingCount} ratings`);

    // DISCOM Data Extraction
    simulateDiscomExtraction(consumerCount);

    // Transco Data Extraction
    setTimeout(() => simulateTranscoExtraction(consumerCount), 1000);

    // Nodal Agency Data Extraction
    setTimeout(() => simulateNodalExtraction(consumerCount), 2000);

    // Rating Agency Extraction
    setTimeout(() => {
        simulateCRISILExtraction(ratingCount);
        simulateICRAExtraction(ratingCount);
        simulateCAREExtraction(ratingCount);
        simulateSPExtraction(ratingCount);
    }, 3000);

    // Final step
    setTimeout(() => {
        addLog('success', 'Data collection completed successfully');
        setStatus('success');
    }, 7000);
}

function simulateDiscomExtraction(count) {
    updateProgressBar('discom-progress', 'discom-text', 0);
    
    let progress = 0;
    const interval = setInterval(() => {
        progress += Math.random() * 30;
        if (progress >= 100) {
            progress = 100;
            clearInterval(interval);
            updateProgressBar('discom-progress', 'discom-text', 100);
            addLog('success', `DISCOM data extraction complete: ${count} records extracted`);
            
            // Generate consumer data
            appState.consumers = generateConsumerData(count);
            displayDataPreview('discom-preview', appState.consumers.slice(0, 3));
        } else {
            updateProgressBar('discom-progress', 'discom-text', Math.min(progress, 100));
        }
    }, 300);
}

function simulateTranscoExtraction(count) {
    updateProgressBar('transco-progress', 'transco-text', 0);
    
    let progress = 0;
    const interval = setInterval(() => {
        progress += Math.random() * 25;
        if (progress >= 100) {
            progress = 100;
            clearInterval(interval);
            updateProgressBar('transco-progress', 'transco-text', 100);
            addLog('success', `Transco data extraction complete: Transmission validation successful`);
            displayDataPreview('transco-preview', generateTranscoData(3));
        } else {
            updateProgressBar('transco-progress', 'transco-text', Math.min(progress, 100));
        }
    }, 400);
}

function simulateNodalExtraction(count) {
    updateProgressBar('nodal-progress', 'nodal-text', 0);
    
    let progress = 0;
    const interval = setInterval(() => {
        progress += Math.random() * 28;
        if (progress >= 100) {
            progress = 100;
            clearInterval(interval);
            updateProgressBar('nodal-progress', 'nodal-text', 100);
            addLog('success', `State Nodal Agency data extraction complete: ${Math.floor(count * 0.7)} renewable projects identified`);
            displayDataPreview('nodal-preview', generateNodalData(3));
        } else {
            updateProgressBar('nodal-progress', 'nodal-text', Math.min(progress, 100));
        }
    }, 350);
}

function simulateCRISILExtraction(count) {
    updateProgressBar('crisil-progress', 'crisil-text', 0);
    
    let progress = 0;
    const interval = setInterval(() => {
        progress += Math.random() * 35;
        if (progress >= 100) {
            progress = 100;
            clearInterval(interval);
            updateProgressBar('crisil-progress', 'crisil-text', 100);
            addLog('success', `CRISIL ratings extracted: ${Math.floor(count * 0.8)} ratings found`);
        } else {
            updateProgressBar('crisil-progress', 'crisil-text', Math.min(progress, 100));
        }
    }, 350);
}

function simulateICRAExtraction(count) {
    updateProgressBar('icra-progress', 'icra-text', 0);
    
    let progress = 0;
    const interval = setInterval(() => {
        progress += Math.random() * 35;
        if (progress >= 100) {
            progress = 100;
            clearInterval(interval);
            updateProgressBar('icra-progress', 'icra-text', 100);
            addLog('success', `ICRA ratings extracted: ${Math.floor(count * 0.75)} ratings found`);
        } else {
            updateProgressBar('icra-progress', 'icra-text', Math.min(progress, 100));
        }
    }, 350);
}

function simulateCAREExtraction(count) {
    updateProgressBar('care-progress', 'care-text', 0);
    
    let progress = 0;
    const interval = setInterval(() => {
        progress += Math.random() * 35;
        if (progress >= 100) {
            progress = 100;
            clearInterval(interval);
            updateProgressBar('care-progress', 'care-text', 100);
            addLog('success', `CARE EDGE ratings extracted: ${Math.floor(count * 0.7)} ratings found`);
        } else {
            updateProgressBar('care-progress', 'care-text', Math.min(progress, 100));
        }
    }, 350);
}

function simulateSPExtraction(count) {
    updateProgressBar('sp-progress', 'sp-text', 0);
    
    let progress = 0;
    const interval = setInterval(() => {
        progress += Math.random() * 35;
        if (progress >= 100) {
            progress = 100;
            clearInterval(interval);
            updateProgressBar('sp-progress', 'sp-text', 100);
            addLog('success', `S&P Global ratings extracted: ${Math.floor(count * 0.65)} ratings found`);
            
            // Generate rating data
            appState.ratings = generateRatingData(count);
        } else {
            updateProgressBar('sp-progress', 'sp-text', Math.min(progress, 100));
        }
    }, 350);
}

// Data Mapping Phase
function runDataMapping() {
    if (appState.consumers.length === 0) {
        addLog('warning', 'Please run data collection first');
        return;
    }

    setStatus('processing');
    addLog('info', 'Starting data mapping and cross-linking');

    // Step 1: Normalization
    simulateNormalization();

    // Step 2: Fuzzy Matching
    setTimeout(() => simulateFuzzyMatching(), 2000);

    // Step 3: Cross-Validation
    setTimeout(() => {
        simulateCrossValidation();
        setStatus('success');
    }, 4000);
}

function simulateNormalization() {
    updateProgressBar('normalize-progress', 'normalize-text', 0);
    
    let progress = 0;
    const interval = setInterval(() => {
        progress += Math.random() * 40;
        if (progress >= 100) {
            progress = 100;
            clearInterval(interval);
            updateProgressBar('normalize-progress', 'normalize-text', 100);
            addLog('success', `Normalization complete: ${appState.consumers.length} consumer records standardized`);
            
            const details = `
                <div class="matching-stat">
                    <span>Legal Entity Names Standardized:</span>
                    <span><strong>${appState.consumers.length}</strong></span>
                </div>
                <div class="matching-stat">
                    <span>Load Values Validated:</span>
                    <span><strong>${appState.consumers.length}</strong></span>
                </div>
                <div class="matching-stat">
                    <span>Geographic Data Verified:</span>
                    <span><strong>${appState.consumers.length}</strong></span>
                </div>
                <div class="matching-stat">
                    <span>Duplicates Detected:</span>
                    <span><strong>${Math.floor(appState.consumers.length * 0.05)}</strong></span>
                </div>
            `;
            document.getElementById('normalize-details').innerHTML = details;
        } else {
            updateProgressBar('normalize-progress', 'normalize-text', Math.min(progress, 100));
        }
    }, 300);
}

function simulateFuzzyMatching() {
    updateProgressBar('matching-progress', 'matching-text', 0);
    
    let progress = 0;
    const interval = setInterval(() => {
        progress += Math.random() * 30;
        if (progress >= 100) {
            progress = 100;
            clearInterval(interval);
            updateProgressBar('matching-progress', 'matching-text', 100);
            
            const exactMatches = Math.floor(appState.consumers.length * 0.6);
            const highConfidence = Math.floor(appState.consumers.length * 0.25);
            const mediumConfidence = Math.floor(appState.consumers.length * 0.1);
            const noMatches = appState.consumers.length - exactMatches - highConfidence - mediumConfidence;
            
            appState.linkages = generateLinkageData(exactMatches, highConfidence, mediumConfidence, noMatches);
            
            addLog('success', `Fuzzy matching complete: ${exactMatches + highConfidence} matches found`);
            
            const stats = `
                <div class="matching-stat">
                    <span>Exact Matches (100%):</span>
                    <span><strong>${exactMatches}</strong></span>
                </div>
                <div class="matching-stat">
                    <span>High Confidence (85-99%):</span>
                    <span><strong>${highConfidence}</strong></span>
                </div>
                <div class="matching-stat">
                    <span>Medium Confidence (70-84%):</span>
                    <span><strong>${mediumConfidence}</strong></span>
                </div>
                <div class="matching-stat">
                    <span>No Match:</span>
                    <span><strong>${noMatches}</strong></span>
                </div>
                <div class="matching-stat">
                    <span>Total Mapping Success Rate:</span>
                    <span><strong>${Math.round(((exactMatches + highConfidence) / appState.consumers.length) * 100)}%</strong></span>
                </div>
            `;
            document.getElementById('matching-stats').innerHTML = stats;
        } else {
            updateProgressBar('matching-progress', 'matching-text', Math.min(progress, 100));
        }
    }, 350);
}

function simulateCrossValidation() {
    updateProgressBar('validation-progress', 'validation-text', 0);
    
    let progress = 0;
    const interval = setInterval(() => {
        progress += Math.random() * 40;
        if (progress >= 100) {
            progress = 100;
            clearInterval(interval);
            updateProgressBar('validation-progress', 'validation-text', 100);
            
            const duplicates = Math.floor(appState.consumers.length * 0.03);
            const conflicts = Math.floor(appState.linkages.length * 0.05);
            const validated = appState.linkages.length - conflicts;
            
            addLog('success', `Cross-validation complete: ${duplicates} duplicates resolved, ${conflicts} conflicts flagged`);
            
            const summary = `
                <div class="matching-stat">
                    <span>Duplicates Detected & Merged:</span>
                    <span><strong>${duplicates}</strong></span>
                </div>
                <div class="matching-stat">
                    <span>Linkages Validated:</span>
                    <span><strong>${validated}</strong></span>
                </div>
                <div class="matching-stat">
                    <span>Conflicts Flagged for Review:</span>
                    <span><strong>${conflicts}</strong></span>
                </div>
                <div class="matching-stat">
                    <span>Validation Success Rate:</span>
                    <span><strong>${Math.round((validated / appState.linkages.length) * 100)}%</strong></span>
                </div>
            `;
            document.getElementById('validation-summary').innerHTML = summary;
        } else {
            updateProgressBar('validation-progress', 'validation-text', Math.min(progress, 100));
        }
    }, 300);
}

// Categorization Phase
function runCategorization() {
    if (appState.linkages.length === 0) {
        addLog('warning', 'Please run data mapping first');
        return;
    }

    setStatus('processing');
    addLog('info', 'Starting data categorization');

    simulateRatingCategorization();
    setTimeout(() => {
        simulateOutlookCategorization();
        simulateSectorCategorization();
        simulateLoadTierCategorization();
        setStatus('success');
        updateDashboard();
    }, 2000);
}

function simulateRatingCategorization() {
    updateProgressBar('category-progress', 'category-text', 0);
    
    let progress = 0;
    const interval = setInterval(() => {
        progress += Math.random() * 40;
        if (progress >= 100) {
            progress = 100;
            clearInterval(interval);
            updateProgressBar('category-progress', 'category-text', 100);
            
            const investmentGrade = Math.floor(appState.linkages.length * 0.4);
            const speculativeGrade = Math.floor(appState.linkages.length * 0.35);
            const belowInvestment = Math.floor(appState.linkages.length * 0.15);
            const unrated = appState.linkages.length - investmentGrade - speculativeGrade - belowInvestment;
            
            appState.results = {
                investmentGrade,
                speculativeGrade,
                belowInvestment,
                unrated,
                totalLoad: Math.round(Math.random() * 5000) + 1000
            };
            
            addLog('success', `Rating categorization complete: ${investmentGrade} investment grade, ${speculativeGrade} speculative`);
            
            const breakdown = `
                <div class="category-item investment-grade">
                    <div class="category-name">Investment Grade (AAA-BBB-)</div>
                    <div class="category-stats">
                        <div class="category-stat-label">Count:</div>
                        <div class="category-stat-value">${investmentGrade}</div>
                        <div class="category-stat-label">% of Total:</div>
                        <div class="category-stat-value">${Math.round((investmentGrade / appState.linkages.length) * 100)}%</div>
                    </div>
                </div>
                <div class="category-item speculative-grade">
                    <div class="category-name">Speculative Grade (BB+-B-)</div>
                    <div class="category-stats">
                        <div class="category-stat-label">Count:</div>
                        <div class="category-stat-value">${speculativeGrade}</div>
                        <div class="category-stat-label">% of Total:</div>
                        <div class="category-stat-value">${Math.round((speculativeGrade / appState.linkages.length) * 100)}%</div>
                    </div>
                </div>
                <div class="category-item below-investment-grade">
                    <div class="category-name">Below Investment (B-D)</div>
                    <div class="category-stats">
                        <div class="category-stat-label">Count:</div>
                        <div class="category-stat-value">${belowInvestment}</div>
                        <div class="category-stat-label">% of Total:</div>
                        <div class="category-stat-value">${Math.round((belowInvestment / appState.linkages.length) * 100)}%</div>
                    </div>
                </div>
                <div class="category-item unrated">
                    <div class="category-name">Unrated</div>
                    <div class="category-stats">
                        <div class="category-stat-label">Count:</div>
                        <div class="category-stat-value">${unrated}</div>
                        <div class="category-stat-label">% of Total:</div>
                        <div class="category-stat-value">${Math.round((unrated / appState.linkages.length) * 100)}%</div>
                    </div>
                </div>
            `;
            document.getElementById('category-breakdown').innerHTML = breakdown;
        } else {
            updateProgressBar('category-progress', 'category-text', Math.min(progress, 100));
        }
    }, 350);
}

function simulateOutlookCategorization() {
    const positive = Math.floor(appState.linkages.length * 0.25);
    const stable = Math.floor(appState.linkages.length * 0.55);
    const negative = Math.floor(appState.linkages.length * 0.15);
    const developing = appState.linkages.length - positive - stable - negative;
    
    addLog('success', `Outlook categorization complete: ${positive} positive, ${stable} stable, ${negative} negative`);
    
    const breakdown = `
        <div class="category-item">
            <div class="category-name">📈 Positive Outlook</div>
            <div class="category-stats">
                <div class="category-stat-label">Count:</div>
                <div class="category-stat-value">${positive}</div>
                <div class="category-stat-label">% of Total:</div>
                <div class="category-stat-value">${Math.round((positive / appState.linkages.length) * 100)}%</div>
            </div>
        </div>
        <div class="category-item">
            <div class="category-name">➡️ Stable Outlook</div>
            <div class="category-stats">
                <div class="category-stat-label">Count:</div>
                <div class="category-stat-value">${stable}</div>
                <div class="category-stat-label">% of Total:</div>
                <div class="category-stat-value">${Math.round((stable / appState.linkages.length) * 100)}%</div>
            </div>
        </div>
        <div class="category-item">
            <div class="category-name">📉 Negative Outlook</div>
            <div class="category-stats">
                <div class="category-stat-label">Count:</div>
                <div class="category-stat-value">${negative}</div>
                <div class="category-stat-label">% of Total:</div>
                <div class="category-stat-value">${Math.round((negative / appState.linkages.length) * 100)}%</div>
            </div>
        </div>
        <div class="category-item">
            <div class="category-name">⏳ Developing/Watch</div>
            <div class="category-stats">
                <div class="category-stat-label">Count:</div>
                <div class="category-stat-value">${developing}</div>
                <div class="category-stat-label">% of Total:</div>
                <div class="category-stat-value">${Math.round((developing / appState.linkages.length) * 100)}%</div>
            </div>
        </div>
    `;
    document.getElementById('outlook-breakdown').innerHTML = breakdown;
}

function simulateSectorCategorization() {
    const sectors = {
        'Solar Renewable': Math.floor(appState.linkages.length * 0.35),
        'Wind Energy': Math.floor(appState.linkages.length * 0.15),
        'Steel & Metals': Math.floor(appState.linkages.length * 0.2),
        'Chemicals': Math.floor(appState.linkages.length * 0.15),
        'Others': 0
    };
    sectors['Others'] = appState.linkages.length - Object.values(sectors).slice(0, -1).reduce((a, b) => a + b, 0);
    
    addLog('success', `Sector categorization complete: ${Object.keys(sectors).length} sectors identified`);
    
    let breakdown = '';
    Object.entries(sectors).forEach(([sector, count]) => {
        breakdown += `
            <div class="category-item">
                <div class="category-name">${sector}</div>
                <div class="category-stats">
                    <div class="category-stat-label">Count:</div>
                    <div class="category-stat-value">${count}</div>
                    <div class="category-stat-label">% of Total:</div>
                    <div class="category-stat-value">${Math.round((count / appState.linkages.length) * 100)}%</div>
                </div>
            </div>
        `;
    });
    document.getElementById('sector-breakdown').innerHTML = breakdown;
}

function simulateLoadTierCategorization() {
    const tier1 = Math.floor(appState.linkages.length * 0.2);
    const tier2 = Math.floor(appState.linkages.length * 0.35);
    const tier3 = appState.linkages.length - tier1 - tier2;
    
    addLog('success', `Load tier categorization complete: ${tier1} >50MWp, ${tier2} 25-50MWp, ${tier3} 10-25MWp`);
    
    const breakdown = `
        <div class="category-item">
            <div class="category-name">>50 MWp (Tier 1)</div>
            <div class="category-stats">
                <div class="category-stat-label">Count:</div>
                <div class="category-stat-value">${tier1}</div>
                <div class="category-stat-label">% of Total:</div>
                <div class="category-stat-value">${Math.round((tier1 / appState.linkages.length) * 100)}%</div>
            </div>
        </div>
        <div class="category-item">
            <div class="category-name">25-50 MWp (Tier 2)</div>
            <div class="category-stats">
                <div class="category-stat-label">Count:</div>
                <div class="category-stat-value">${tier2}</div>
                <div class="category-stat-label">% of Total:</div>
                <div class="category-stat-value">${Math.round((tier2 / appState.linkages.length) * 100)}%</div>
            </div>
        </div>
        <div class="category-item">
            <div class="category-name">10-25 MWp (Tier 3)</div>
            <div class="category-stats">
                <div class="category-stat-label">Count:</div>
                <div class="category-stat-value">${tier3}</div>
                <div class="category-stat-label">% of Total:</div>
                <div class="category-stat-value">${Math.round((tier3 / appState.linkages.length) * 100)}%</div>
            </div>
        </div>
    `;
    document.getElementById('load-tier-breakdown').innerHTML = breakdown;
}

// Export Results
function exportResults(format) {
    if (appState.linkages.length === 0) {
        addLog('warning', 'No results to export. Please complete the test run first.');
        return;
    }

    addLog('info', `Exporting results as ${format.toUpperCase()}`);

    if (format === 'json') {
        exportJSON();
    } else if (format === 'csv') {
        exportCSV();
    }

    addLog('success', `Results exported successfully as ${format.toUpperCase()}`);
}

function exportJSON() {
    const data = {
        metadata: {
            exportDate: new Date().toISOString(),
            totalConsumers: appState.consumers.length,
            totalRatings: appState.ratings.length,
            totalLinkages: appState.linkages.length
        },
        consumers: appState.consumers,
        ratings: appState.ratings,
        linkages: appState.linkages,
        categorization: appState.results
    };

    const dataStr = JSON.stringify(data, null, 2);
    downloadFile(dataStr, 'test-results.json', 'application/json');
}

function exportCSV() {
    let csv = 'Consumer ID,Legal Entity Name,State,Sanctioned Load,CRISIL Rating,ICRA Rating,CARE Rating,SP Rating,Match Confidence\n';
    
    appState.linkages.forEach(linkage => {
        csv += `${linkage.consumerId},${linkage.consumerName},${linkage.state},${linkage.load},${linkage.crisil},${linkage.icra},${linkage.care},${linkage.sp},${linkage.confidence}\n`;
    });

    downloadFile(csv, 'test-results.csv', 'text/csv');
}

function downloadFile(content, filename, type) {
    const blob = new Blob([content], { type });
    const url = window.URL.createObjectURL(blob);
    const link = document.createElement('a');
    link.href = url;
    link.download = filename;
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
    window.URL.revokeObjectURL(url);
}

// Clear Data
function clearAll() {
    if (confirm('Are you sure you want to clear all data? This cannot be undone.')) {
        appState = {
            consumers: [],
            ratings: [],
            linkages: [],
            results: {},
            logs: [],
            startTime: null
        };

        // Reset UI
        document.querySelectorAll('.progress-fill').forEach(el => {
            el.style.width = '0%';
        });
        document.querySelectorAll('.progress-text').forEach(el => {
            el.textContent = 'Ready';
        });

        updateDashboard();
        addLog('info', 'All data cleared');
        setStatus('Ready');
    }
}

// Update Dashboard
function updateDashboard() {
    const mappedCount = appState.linkages.filter(l => l.confidence > 60).length;
    const successRate = appState.consumers.length > 0 ? 
        Math.round((mappedCount / appState.consumers.length) * 100) : 0;

    document.getElementById('totalConsumers').textContent = appState.consumers.length;
    document.getElementById('mappedConsumers').textContent = mappedCount;
    document.getElementById('mappingSuccess').textContent = successRate + '%';

    if (appState.startTime) {
        const elapsed = Math.round((Date.now() - appState.startTime) / 1000);
        document.getElementById('processingTime').textContent = elapsed + 's';
    }

    // State-wise distribution
    const states = {
        'Haryana': appState.consumers.filter(c => c.state === 'Haryana').length,
        'Uttar Pradesh': appState.consumers.filter(c => c.state === 'Uttar Pradesh').length,
        'Karnataka': appState.consumers.filter(c => c.state === 'Karnataka').length,
        'Uttarakhand': appState.consumers.filter(c => c.state === 'Uttarakhand').length
    };

    const maxState = Math.max(...Object.values(states));
    Object.entries(states).forEach(([state, count]) => {
        const percentage = maxState > 0 ? (count / maxState) * 100 : 0;
        const stateKey = state.toLowerCase().replace(' ', '-');
        document.getElementById(`${stateKey}-bar`).style.width = percentage + '%';
        document.getElementById(`${stateKey}-count`).textContent = count;
    });

    // Load tier distribution
    const tier1 = appState.consumers.filter(c => c.load > 50).length;
    const tier2 = appState.consumers.filter(c => c.load > 25 && c.load <= 50).length;
    const tier3 = appState.consumers.filter(c => c.load >= 10 && c.load <= 25).length;

    document.getElementById('tier1-count').textContent = tier1;
    document.getElementById('tier2-count').textContent = tier2;
    document.getElementById('tier3-count').textContent = tier3;
}

// Logging
function addLog(level, message) {
    const timestamp = new Date().toLocaleTimeString();
    appState.logs.push({ level, message, timestamp });

    const logsContainer = document.getElementById('logsContainer');
    const logEntry = document.createElement('div');
    logEntry.className = 'log-entry';
    logEntry.innerHTML = `
        <span class="log-timestamp">${timestamp}</span>
        <span class="log-level ${level}">${level.toUpperCase()}</span>
        <span class="log-message">${message}</span>
    `;
    logsContainer.appendChild(logEntry);
    logsContainer.scrollTop = logsContainer.scrollHeight;
}

function clearLogs() {
    appState.logs = [];
    document.getElementById('logsContainer').innerHTML = '';
    addLog('info', 'Logs cleared');
}

function filterLogs() {
    const level = document.getElementById('logLevel').value;
    const entries = document.querySelectorAll('.log-entry');
    
    entries.forEach(entry => {
        if (level === 'all' || entry.querySelector(`.log-level.${level}`)) {
            entry.style.display = 'flex';
        } else {
            entry.style.display = 'none';
        }
    });
}

// Utility Functions
function updateProgressBar(progressId, textId, percentage) {
    const progressFill = document.getElementById(progressId);
    progressFill.style.width = percentage + '%';
    
    let status = 'Processing...';
    if (percentage === 100) {
        status = 'Complete ✓';
    }
    document.getElementById(textId).textContent = status;
}

function displayDataPreview(elementId, data) {
    const preview = document.getElementById(elementId);
    preview.textContent = JSON.stringify(data, null, 2);
}

function setStatus(status) {
    const badge = document.getElementById('statusBadge');
    badge.textContent = status.charAt(0).toUpperCase() + status.slice(1);
    badge.className = 'status-badge ' + status;
}
```

Continue in next message...

---

## 4. Test Data Generator (test-data.js)

```javascript
// Generate test consumer data
function generateConsumerData(count) {
    const states = ['Haryana', 'Uttar Pradesh', 'Karnataka', 'Uttarakhand'];
    const categories = ['HT-I', 'HT-II', 'HT-III', 'LT-IV'];
    const sectors = ['Solar Renewable', 'Wind Energy', 'Steel & Metals', 'Chemicals', 'Manufacturing'];
    const discoms = {
        'Haryana': ['DHBCL', 'UHBCL'],
        'Uttar Pradesh': ['UPPCL', 'UPWCL'],
        'Karnataka': ['KPDCL'],
        'Uttarakhand': ['UPCL']
    };

    const consumers = [];
    for (let i = 0; i < count; i++) {
        const state = states[Math.floor(Math.random() * states.length)];
        consumers.push({
            consumerId: `C${String(i + 1).padStart(6, '0')}`,
            consumerName: `Consumer ${i + 1}`,
            legalEntityName: `Legal Entity ${String(i + 1).padStart(4, '0')} Pvt Ltd`,
            state,
            district: `District-${Math.floor(Math.random() * 10) + 1}`,
            discom: discoms[state][Math.floor(Math.random() * discoms[state].length)],
            load: Math.round((Math.random() * 90 + 10) * 10) / 10,
            category: categories[Math.floor(Math.random() * categories.length)],
            sector: sectors[Math.floor(Math.random() * sectors.length)],
            connectionDate: `2020-${String(Math.floor(Math.random() * 12) + 1).padStart(2, '0')}-${String(Math.floor(Math.random() * 28) + 1).padStart(2, '0')}`,
            address: `Address ${i + 1}, ${state}`
        });
    }
    return consumers;
}

// Generate transco data
function generateTranscoData(count) {
    const data = [];
    for (let i = 0; i < count; i++) {
        data.push({
            feederCode: `FDR-${String(i + 1).padStart(4, '0')}`,
            substationCode: `SUB-${String(Math.floor(Math.random() * 50) + 1).padStart(3, '0')}`,
            loadProfile: `${Math.round(Math.random() * 90 + 10)} MWp`,
            transmissionLoss: `${Math.round(Math.random() * 5 * 10) / 10}%`
        });
    }
    return data;
}

// Generate nodal agency data
function generateNodalData(count) {
    const technologies = ['Solar PV', 'Wind', 'Hybrid', 'Thermal'];
    const data = [];
    for (let i = 0; i < count; i++) {
        data.push({
            projectCode: `PROJ-${String(i + 1).padStart(5, '0')}`,
            technology: technologies[Math.floor(Math.random() * technologies.length)],
            capacity: `${Math.round(Math.random() * 90 + 10)} MWp`,
            status: Math.random() > 0.3 ? 'Operational' : 'Under Construction'
        });
    }
    return data;
}

// Generate rating data
function generateRatingData(count) {
    const grades = ['AAA', 'AA+', 'AA', 'AA-', 'A+', 'A', 'A-', 'BBB+', 'BBB', 'BBB-', 'BB+', 'BB', 'BB-', 'B+', 'B', 'B-', 'C', 'D'];
    const outlooks = ['Positive', 'Stable', 'Negative', 'Developing'];
    const ratings = [];

    for (let i = 0; i < count; i++) {
        ratings.push({
            ratingId: `R${String(i + 1).padStart(6, '0')}`,
            agency: ['CRISIL', 'ICRA', 'CARE EDGE', 'S&P Global'][Math.floor(Math.random() * 4)],
            grade: grades[Math.floor(Math.random() * grades.length)],
            outlook: outlooks[Math.floor(Math.random() * outlooks.length)],
            ratingDate: `2024-${String(Math.floor(Math.random() * 12) + 1).padStart(2, '0')}-${String(Math.floor(Math.random() * 28) + 1).padStart(2, '0')}`
        });
    }
    return ratings;
}

// Generate linkage data
function generateLinkageData(exact, high, medium, noMatch) {
    const linkages = [];
    const grades = ['AAA', 'AA+', 'AA', 'AA-', 'A+', 'A', 'A-', 'BBB+', 'BBB', 'BBB-', 'BB+', 'BB', 'BB-', 'B+', 'B', 'B-', 'C', 'D'];
    
    let id = 0;

    // Exact matches
    for (let i = 0; i < exact; i++) {
        linkages.push({
            linkageId: `L${String(id++).padStart(7, '0')}`,
            consumerId: `C${String(i).padStart(6, '0')}`,
            consumerName: `Consumer ${i}`,
            state: ['Haryana', 'Uttar Pradesh', 'Karnataka', 'Uttarakhand'][Math.floor(Math.random() * 4)],
            load: Math.round((Math.random() * 90 + 10) * 10) / 10,
            crisil: grades[Math.floor(Math.random() * grades.length)],
            icra: grades[Math.floor(Math.random() * grades.length)],
            care: grades[Math.floor(Math.random() * grades.length)],
            sp: grades[Math.floor(Math.random() * grades.length)],
            confidence: 100
        });
    }

    // High confidence matches
    for (let i = 0; i < high; i++) {
        linkages.push({
            linkageId: `L${String(id++).padStart(7, '0')}`,
            consumerId: `C${String(exact + i).padStart(6, '0')}`,
            consumerName: `Consumer ${exact + i}`,
            state: ['Haryana', 'Uttar Pradesh', 'Karnataka', 'Uttarakhand'][Math.floor(Math.random() * 4)],
            load: Math.round((Math.random() * 90 + 10) * 10) / 10,
            crisil: grades[Math.floor(Math.random() * grades.length)],
            icra: grades[Math.floor(Math.random() * grades.length)],
            care: grades[Math.floor(Math.random() * grades.length)],
            sp: grades[Math.floor(Math.random() * grades.length)],
            confidence: Math.round(Math.random() * 14 + 86)
        });
    }

    // Medium confidence matches
    for (let i = 0; i < medium; i++) {
        linkages.push({
            linkageId: `L${String(id++).padStart(7, '0')}`,
            consumerId: `C${String(exact + high + i).padStart(6, '0')}`,
            consumerName: `Consumer ${exact + high + i}`,
            state: ['Haryana', 'Uttar Pradesh', 'Karnataka', 'Uttarakhand'][Math.floor(Math.random() * 4)],
            load: Math.round((Math.random() * 90 + 10) * 10) / 10,
            crisil: grades[Math.floor(Math.random() * grades.length)],
            icra: 'N/A',
            care: grades[Math.floor(Math.random() * grades.length)],
            sp: 'N/A',
            confidence: Math.round(Math.random() * 14 + 70)
        });
    }

    // No matches (unrated)
    for (let i = 0; i < noMatch; i++) {
        linkages.push({
            linkageId: `L${String(id++).padStart(7, '0')}`,
            consumerId: `C${String(exact + high + medium + i).padStart(6, '0')}`,
            consumerName: `Consumer ${exact + high + medium + i}`,
            state: ['Haryana', 'Uttar Pradesh', 'Karnataka', 'Uttarakhand'][Math.floor(Math.random() * 4)],
            load: Math.round((Math.random() * 90 + 10) * 10) / 10,
            crisil: 'UNRATED',
            icra: 'UNRATED',
            care: 'UNRATED',
            sp: 'UNRATED',
            confidence: 0
        });
    }

    return linkages;
}
```

---

## 5. Utility Functions (utils.js)

```javascript
// Utility functions for the application

// String similarity calculator (Levenshtein distance)
function calculateSimilarity(str1, str2) {
    const s1 = str1.toLowerCase().trim();
    const s2 = str2.toLowerCase().trim();
    
    if (s1 === s2) return 100;
    
    const longer = s1.length > s2.length ? s1 : s2;
    const shorter = s1.length > s2.length ? s2 : s1;
    
    if (longer.length === 0) return 100;
    
    const editDistance = getEditDistance(longer, shorter);
    return ((longer.length - editDistance) / longer.length) * 100;
}

function getEditDistance(s1, s2) {
    const costs = [];
    for (let i = 0; i <= s1.length; i++) {
        let lastValue = i;
        for (let j = 0; j <= s2.length; j++) {
            if (i === 0) {
                costs[j] = j;
            } else if (j > 0) {
                let newValue = costs[j - 1];
                if (s1.charAt(i - 1) !== s2.charAt(j - 1)) {
                    newValue = Math.min(Math.min(newValue, lastValue), costs[j]) + 1;
                }
                costs[j - 1] = lastValue;
                lastValue = newValue;
            }
        }
        if (i > 0) costs[s2.length] = lastValue;
    }
    return costs[s2.length];
}

// Normalize text for matching
function normalizeText(text) {
    return text
        .toLowerCase()
        .trim()
        .replace(/[^\w\s]/g, '')
        .replace(/\s+/g, ' ')
        .replace(/pvt\s+ltd|private\s+limited|limited|ltd|inc|corporation|corp/gi, '')
        .trim();
}

// Validate email
function isValidEmail(email) {
    const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return re.test(email);
}

// Format number with thousands separator
function formatNumber(num) {
    return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
}

// Format date
function formatDate(dateString) {
    const options = { year: 'numeric', month: 'short', day: 'numeric' };
    return new Date(dateString).toLocaleDateString('en-US', options);
}

// Get rating category
function getRatingCategory(grade) {
    const investmentGrades = ['AAA', 'AA+', 'AA', 'AA-', 'A+', 'A', 'A-', 'BBB+', 'BBB', 'BBB-'];
    const speculativeGrades = ['BB+', 'BB', 'BB-', 'B+', 'B', 'B-'];
    const belowInvestmentGrades = ['C', 'D'];

    if (investmentGrades.includes(grade)) return 'Investment Grade';
    if (speculativeGrades.includes(grade)) return 'Speculative Grade';
    if (belowInvestmentGrades.includes(grade)) return 'Below Investment Grade';
    return 'Unrated';
}

// Get rating numeric value
function getRatingNumeric(grade) {
    const ratingMap = {
        'AAA': 10, 'AA+': 9.5, 'AA': 9, 'AA-': 8.5,
        'A+': 8, 'A': 7.5, 'A-': 7,
        'BBB+': 6.5, 'BBB': 6, 'BBB-': 5.5,
        'BB+': 5, 'BB': 4.5, 'BB-': 4,
        'B+': 3.5, 'B': 3, 'B-': 2.5,
        'C': 1.5, 'D': 1
    };
    return ratingMap[grade] || 0;
}

// Calculate average rating
function calculateAverageRating(ratings) {
    const validRatings = ratings
        .filter(r => r !== 'UNRATED' && r !== 'N/A')
        .map(r => getRatingNumeric(r));
    
    if (validRatings.length === 0) return 'N/A';
    
    const average = validRatings.reduce((a, b) => a + b, 0) / validRatings.length;
    return average.toFixed(2);
}

// Validate load value
function isValidLoad(load) {
    return load > 10 && load < 1000;
}

// Get random item from array
function getRandomItem(array) {
    return array[Math.floor(Math.random() * array.length)];
}

// Deep clone object
function deepClone(obj) {
    return JSON.parse(JSON.stringify(obj));
}

// Check if object is empty
function isEmpty(obj) {
    return Object.keys(obj).length === 0;
}

// Get keys from object as array
function getKeys(obj) {
    return Object.keys(obj);
}

// Sort array of objects
function sortByKey(array, key, ascending = true) {
    return array.sort((a, b) => {
        if (ascending) {
            return a[key] > b[key] ? 1 : -1;
        } else {
            return a[key] < b[key] ? 1 : -1;
        }
    });
}

// Filter array of objects
function filterByKey(array, key, value) {
    return array.filter(obj => obj[key] === value);
}

// Group array by key
function groupByKey(array, key) {
    return array.reduce((result, obj) => {
        const group = obj[key];
        if (!result[group]) {
            result[group] = [];
        }
        result[group].push(obj);
        return result;
    }, {});
}

// Debounce function
function debounce(func, wait) {
    let timeout;
    return function executedFunction(...args) {
        const later = () => {
            clearTimeout(timeout);
            func(...args);
        };
        clearTimeout(timeout);
        timeout = setTimeout(later, wait);
    };
}

// Throttle function
function throttle(func, limit) {
    let inThrottle;
    return function(...args) {
        if (!inThrottle) {
            func.apply(this, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
}
```

---

## Usage Instructions

### To Run the Test:

1. **Create project structure:**
   ```bash
   mkdir test-run-frontend
   cd test-run-frontend
   ```

2. **Create files:**
   - Save HTML as `index.html`
   - Save CSS as `styles.css`
   - Save main JS as `app.js`
   - Save test data JS as `test-data.js`
   - Save utilities as `utils.js`

3. **Open in browser:**
   ```bash
   # Using Python 3
   python -m http.server 8000
   
   # Or using Python 2
   python -m SimpleHTTPServer 8000
   
   # Then visit: http://localhost:8000
   ```

4. **Run test phases:**
   - Go to "Data Collection" tab → Set parameters → Click "Start Data Collection"
   - Go to "Data Mapping" tab → Click "Start Data Mapping"
   - Go to "Categorization" tab → Click "Start Categorization"
   - View results in "Results" tab
   - Check logs in "Logs" tab

### Features:

✅ **No dependencies** - Pure HTML, CSS, JavaScript  
✅ **Complete workflow** - All 3 phases with simulated data  
✅ **Real-time dashboard** - Live metrics and statistics  
✅ **Progress tracking** - Visual progress bars for each step  
✅ **Result export** - JSON and CSV export  
✅ **Comprehensive logging** - Detailed activity logs  
✅ **Responsive design** - Works on desktop and mobile  

---

**Version**: 1.0  
**Status**: Ready for Testing
