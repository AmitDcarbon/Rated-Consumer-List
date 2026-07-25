# Consumer Credit Rating Data Scraping Agent

## Project Overview
Automated agent for scraping and categorizing large industrial consumers with credit ratings from authorized rating agencies. Focuses on renewable energy consumers in select Indian states.

---

## 1. Data Sources & Scope

### 1.1 Authorized Credit Rating Agencies
- **CRISIL** (Credit Rating Information Services of India Limited)
- **ICRA** (Investment Information and Credit Rating Agency of India Limited)
- **CARE EDGE** (Credit Analysis & Research Limited)
- **S&P Global** (Standard & Poor's Global Ratings)

### 1.2 Geographic Scope (State-wise Priority)
1. **Haryana**
2. **Uttar Pradesh (UP)**
3. **Karnataka**
4. **Uttarakhand**

### 1.3 Consumer Eligibility Criteria
- **Minimum Sanctioned Load**: > 10 MWp (Megawatt Peak)
- **Consumer Category**: Large Industrial Consumers
- **Data Mapping**: Cross-linked with DISCOM records
- **Validation Sources**: DISCOM, Transco, State Nodal Agency

---

## 2. Primary Data Sources

### 2.1 DISCOM (Distribution Company) Sources
- **Haryana**: Dakshin Haryana Bijli Company Limited (DHBCL), Uttar Haryana Bijli Company Limited (UHBCL)
- **Uttar Pradesh**: Uttar Pradesh Poorv Kshetra Vidyut Company Limited (UPPCL), Uttar Pradesh Paschim Kshetra Vidyut Company Limited (UPWCL), Madhya Pradesh Poorv Kshetra Vidyut Company Limited (MPPGCL)
- **Karnataka**: Karnataka Power Distribution Company Limited (KPDCL)
- **Uttarakhand**: Uttarakhand Power Distribution Company Limited (UPCL)

### 2.2 Transco (Transmission Company) Sources
- **POWERGRID** - National Power Grid Corporation of India Limited
- **State Transmission Companies** (STU)
  - Haryana Vidyut Transmission Company Limited (HVTCL)
  - Uttar Pradesh Power Transmission Corporation Limited (UPPTCL)
  - Karnataka Power Transmission Corporation Limited (KPTCL)
  - Uttarakhand Power Transmission Corporation Limited (UPTCL)

### 2.3 State Nodal Agencies
- **MNRE - Ministry of New and Renewable Energy** (National level)
- **State Renewable Energy Agencies**:
  - Haryana New and Renewable Energy Corporation (HNREC)
  - Uttar Pradesh New and Renewable Energy Development Agency (UPNEDA)
  - Karnataka Renewable Energy Development Limited (KREDL)
  - Uttarakhand Renewable Energy Development Agency (UREDA)

---

## 3. Data Collection Workflow

### Phase 1: Consumer Data Extraction

#### Step 1.1: Extract from DISCOM Consumer Lists
```
For each State:
├── Access DISCOM Portal / Public Reports
├── Filter: Large Consumers (>10 MWp sanctioned load)
├── Extract Fields:
│   ├── Consumer ID
│   ├── Consumer Name
│   ├── Sanctioned Load (MW)
│   ├── Category (HT-I, HT-II, etc.)
│   ├── Supply Voltage
│   ├── Legal Entity Name
│   ├── Registration/Incorporation Number
│   ├── Address (Full)
│   ├── State / District / Taluk
│   ├── Contact Information
│   └── Connected Since (Date)
└── Store in: discom_consumers_raw.csv
```

#### Step 1.2: Extract from Transco Databases
```
For each State Transmission Company:
├── Access Transco Portal / Public Database
├── Cross-reference with DISCOM data
├── Validate: Sanctioned Load & Consumer ID
├── Extract Fields:
│   ├── Feeder Code
│   ├── Substation Code
│   ├── Connection Reference
│   ├── Load Profile Data
│   └── Transmission Loss Data
└── Merge with: discom_consumers_raw.csv
```

#### Step 1.3: Extract from State Nodal Agency Records
```
For each State Renewable Energy Agency:
├── Access Government Portal / Records
├── Identify: Renewable Energy Consumers
├── Filter: Grid-connected, >10 MWp capacity
├── Extract Fields:
│   ├── Project Code
│   ├── Technology (Solar, Wind, Hybrid, etc.)
│   ├── Installed Capacity (MW)
│   ├── Project Status
│   ├── Developer/Consumer Legal Name
│   ├── Developer Registration
│   ├── Land Details
│   └── Interconnection Point
└── Link to: Consumer Legal Entity Name
```

### Phase 2: Rating Agency Data Scraping

#### Step 2.1: CRISIL Database Extraction
```
Access: https://www.crisil.com/en/home/crisil-ratings

For each Target State:
├── Search: Rated Companies in State
├── Filter by Industry: Power, Infrastructure, Manufacturing
├── Extract Fields:
│   ├── Company Legal Name
│   ├── CRISIL Rating (Grade & Outlook)
│   ├── Rating Date
│   ├── Industry Sector
│   ├── Revenue (Last FY)
│   ├── Debt Profile
│   ├── Rating Rationale (Summary)
│   ├── Contacts
│   └── Last Update Date
└── Store in: crisil_ratings.csv
```

#### Step 2.2: ICRA Database Extraction
```
Access: https://www.icraindia.com/ratings

For each Target State:
├── Search: Rated Companies in State
├── Filter by Industry: Power, Energy, Utilities
├── Extract Fields:
│   ├── Company Legal Name
│   ├── ICRA Rating (Grade & Outlook)
│   ├── Rating Date
│   ├── Industry Classification
│   ├── Annual Revenue
│   ├── Financial Metrics (Debt/EBITDA, Interest Coverage)
│   ├── Rating Methodology Applied
│   ├── Regulatory Environment Notes
│   └── Last Review Date
└── Store in: icra_ratings.csv
```

#### Step 2.3: CARE EDGE Database Extraction
```
Access: https://www.careratings.com

For each Target State:
├── Search: Rated Companies in State
├── Filter by Sector: Power & Utilities, Industrial
├── Extract Fields:
│   ├── Company Legal Name
│   ├── CARE EDGE Rating (Grade & Outlook)
│   ├── Rating Date
│   ├── Sector/Industry Code
│   ├── Operating Performance Indicators
│   ├── Leverage Metrics
│   ├── Rating Drivers (Strengths & Weaknesses)
│   ├── Key Person Information
│   └── Surveillance Status
└── Store in: care_edge_ratings.csv
```

#### Step 2.4: S&P Global Database Extraction
```
Access: https://www.spglobal.com/ratings

For each Target State:
├── Search: Rated Companies / Issuers in State
├── Filter by Sector: Power, Energy, Industrial Manufacturing
├── Extract Fields:
│   ├── Company Legal Name
│   ├── S&P Rating (Letter Grade & Outlook)
│   ├── Rating Date
│   ├── Sector Classification
│   ├── Scale: National / International
│   ├── Financial Profile (Revenue, EBITDA)
│   ├── Credit Strengths & Concerns
│   ├── Key Business Risks
│   ├── Related Party Info
│   └── Last Rating Action Date
└── Store in: sp_global_ratings.csv
```

---

## 4. Data Mapping & Cross-Linking

### Step 4.1: Consumer Normalization
```
Input: discom_consumers_raw.csv + nodal_agency_data.csv

Process:
├── Standardize Legal Entity Names
├── Validate Sanctioned Load
├── Geocode Addresses
└── Output: normalized_consumers.csv
```

### Step 4.2: Rating Agency Data Normalization
```
Inputs: All agency rating CSVs

Process:
├── Standardize Company Names
├── Standardize Rating Scales (AAA-D)
├── Validate Rating Dates
└── Output: normalized_ratings.csv
```

### Step 4.3: Primary Mapping (Consumer ↔ Rating)
```
Algorithm: Fuzzy String Matching + Business Logic

For each Consumer:
├── Search Rating Databases for Legal Entity Name
├── Exact Match (Score: 100%)
├── Fuzzy Match (Score: >85%)
├── Partial Match (Score: 60-85%)
├── No Match (Score: 0%)
└── Output: Linked Records
```

### Step 4.4: Cross-Validation & De-duplication
```
Process:
├── Identify Duplicate Consumers
├── Resolve Rating Conflicts
├── Document All Conflicts
└── Output: validated_consumer_ratings.csv
```

---

## 5. Data Categorization Framework

### 5.1 Rating-Based Categorization

#### Category 1: Investment Grade (AAA to BBB-)
- Low Default Risk
- Strong Financial Position
- Reliable Payment History

#### Category 2: Speculative Grade (BB+ to B-)
- Moderate Default Risk
- Volatile Financial Performance
- Moderate Payment Consistency

#### Category 3: Below Investment Grade (B to D)
- High Default Risk
- Weak Financial Position
- Payment Issues or Default History

#### Category 4: Unrated Consumers
- No available credit rating
- Limited financial disclosure
- Unknown Credit Risk

### 5.2 Outlook-Based Categorization

#### Positive Outlook
- Expected upgrade within 12-24 months
- Growing Financial Strength

#### Stable Outlook
- Stability in credit quality expected
- Consistent Financial Performance

#### Negative Outlook
- Expected downgrade within 12-24 months
- Deteriorating Financial Metrics

#### Developing/Watch Outlook
- Active review for rating change
- Outcome Uncertain

### 5.3 Sector-Based Categorization

```
Primary Sectors:
├── Power Generation
│   ├── Solar Renewable Energy Developers
│   ├── Wind Energy Developers
│   ├── Hybrid Renewable Energy Systems
│   └── Thermal Power Generators
├── Industrial Manufacturing
│   ├── Steel & Metals
│   ├── Chemicals & Petrochemicals
│   ├── Cement & Building Materials
│   ├── Automotive & Components
│   ├── Textiles & Apparel
│   ├── Electronics & IT Hardware
│   └── Other Manufacturing
├── Infrastructure & Utilities
│   ├── Water Treatment & Supply
│   ├── Wastewater Management
│   ├── Roads & Transportation
│   └── Data Centers & Telecom
└── Other Industrial Consumers
```

### 5.4 Geographic Categorization

#### By State (4 Target States)
```
1. Haryana
   ├── Consumers Count: [To be populated]
   ├── Investment Grade: [To be populated]
   ├── Total Sanctioned Load: [To be populated] MWp

2. Uttar Pradesh
   ├── Consumers Count: [To be populated]
   ├── Investment Grade: [To be populated]
   ├── Total Sanctioned Load: [To be populated] MWp

3. Karnataka
   ├── Consumers Count: [To be populated]
   ├── Investment Grade: [To be populated]
   ├── Total Sanctioned Load: [To be populated] MWp

4. Uttarakhand
   ├── Consumers Count: [To be populated]
   ├── Investment Grade: [To be populated]
   ├── Total Sanctioned Load: [To be populated] MWp
```

### 5.5 Load-Based Categorization

```
Load Tier 1: >50 MWp
Load Tier 2: 25-50 MWp
Load Tier 3: 10-25 MWp
```

---

## 6. Data Quality & Validation Rules

### 6.1 Consumer Data Validation
- Sanctioned Load: > 10 MWp (MANDATORY)
- Legal Entity Name: Valid company registration
- State: Haryana, UP, Karnataka, or Uttarakhand
- Duplicate Detection: Same name + state = duplicate

### 6.2 Rating Data Validation
- Rating Grade: AAA to D (valid grades only)
- Rating Date: Within last 5 years
- Outlook: Positive, Stable, Negative, or Developing
- Agency Consistency: Flag divergence >3 notches

---

## 7. Output Data Structure

### 7.1 Master Consumer Registry (CSV)
```
Consumer_ID, Legal_Entity_Name, State, District, Sanctioned_Load_MWp,
Consumer_Category, Connection_Date, Address_Full, Primary_Sector
```

### 7.2 Credit Rating Master Table (CSV)
```
Rating_Record_ID, Legal_Entity_Name, Rating_Agency, Rating_Grade,
Outlook, Rating_Date, Sector_Classification, Annual_Revenue_INR_Lakhs
```

### 7.3 Consumer-Rating Linkage Table (CSV)
```
Linkage_ID, Consumer_ID, Legal_Entity_Name_Consumer, CRISIL_Rating,
ICRA_Rating, CARE_EDGE_Rating, S&P_Rating, Average_Rating_Numeric,
Rating_Category, Outlook_Consensus, Match_Confidence_Score, State, Load_MWp
```

### 7.4 Summary Statistics Report (JSON)
- Total consumers identified
- Consumers mapped to ratings
- Mapping success rate
- Rating distribution
- Outlook distribution
- State-wise breakdown
- Sector-wise breakdown
- Load tier analysis

### 7.5 Quality Metrics Report (JSON)
- Data completeness
- Accuracy metrics
- Consistency metrics
- Data currency metrics
- Validation summary
- Mapping quality
- Data freshness

---

## 8. Execution Timeline

### Phase 1: Setup & Source Identification (Week 1-2)
- [ ] Identify DISCOM public portals
- [ ] Identify Transco data sources
- [ ] Identify State Nodal Agency websites
- [ ] Create access credentials
- [ ] Develop scraping scripts

### Phase 2: Data Collection (Week 3-6)
- [ ] Extract consumer data from all 4 states
- [ ] Cross-validate and consolidate

### Phase 3: Rating Agency Scraping (Week 7-10)
- [ ] Scrape all 4 rating agencies
- [ ] Normalize and validate

### Phase 4: Data Mapping & Linking (Week 11-13)
- [ ] Execute fuzzy matching
- [ ] Manual verification
- [ ] Generate linkage table

### Phase 5: Categorization & Reporting (Week 14-15)
- [ ] Generate final reports
- [ ] Create summaries

### Phase 6: Review & Deployment (Week 16)
- [ ] Final QA
- [ ] Deploy to repository

---

## 9. Technical Specifications

### 9.1 Required Libraries (Python 3.9+)
```
Data Collection:
- requests
- beautifulsoup4
- selenium
- scrapy

Data Processing:
- pandas
- numpy
- pyarrow

Data Matching:
- fuzzywuzzy
- python-Levenshtein
- rapidfuzz

Geospatial:
- geopy
- folium

Utilities:
- sqlalchemy
- logging
- configparser
```

### 9.2 Data Storage Structure
```
├── /data/raw/
│   ├── discom_consumers_*.csv
│   ├── transco_data_*.csv
│   ├── nodal_agency_data_*.csv
│   ├── *_ratings.csv

├── /data/processed/
│   ├── normalized_consumers.csv
│   ├── normalized_ratings.csv
│   ├── validated_consumer_ratings.csv
│   └── conflict_log.csv

├── /data/output/
│   ├── master_consumer_registry.csv
│   ├── credit_rating_master.csv
│   ├── consumer_rating_linkage.csv
│   ├── summary_statistics.json
│   └── quality_metrics.json

└── /logs/
    ├── scraping_log.txt
    ├── processing_log.txt
    ├── validation_log.txt
    └── matching_log.txt
```

---

## 10. Reference URLs

### 10.1 DISCOM Portals
```
Haryana:
- DHBCL: https://www.ddhbcl.com/
- UHBCL: https://www.udhbcl.com/

Uttar Pradesh:
- UPPCL: https://www.uppcl.org/
- UPWCL: https://www.upwcl.org/
- MPPGCL: https://www.mppgcl.nic.in/

Karnataka:
- KPDCL: https://www.kptcl.com/

Uttarakhand:
- UPCL: https://www.ujvnl.org/
```

### 10.2 Rating Agency Websites
```
CRISIL: https://www.crisil.com/
ICRA: https://www.icraindia.com/
CARE EDGE: https://www.careratings.com/
S&P Global: https://www.spglobal.com/ratings/
```

### 10.3 State Nodal Agency Links
```
MNRE: https://mnre.gov.in/

Haryana: https://www.hnrec.gov.in/
UP: https://upneda.org/
Karnataka: https://kredl.karnataka.gov.in/
Uttarakhand: https://ureda.uk.gov.in/
```

---

## 11. Contact & Support

### Data Governance
- **Data Owner**: [To be assigned]
- **Data Steward**: [To be assigned]
- **Technical Lead**: [To be assigned]
- **QA Lead**: [To be assigned]

### Update Frequency
- **Consumer Data**: Quarterly
- **Rating Data**: Monthly
- **Linkage Records**: After each update
- **Summary Reports**: Monthly
- **Quality Reports**: Weekly

### Issue Escalation
- **Critical Issues**: Immediate escalation
- **High Priority**: 24 hours
- **Medium Priority**: 1 week
- **Low Priority**: Backlog

---

**Document Version**: 1.0  
**Last Updated**: 2026-07-25  
**Next Review**: 2026-10-25  
**Status**: Active
