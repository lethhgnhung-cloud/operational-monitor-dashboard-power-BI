🎯 **Project Objective**
Previously, the organization managed operational expenses through fragmented, manual monthly reports. This led to significant delays in detecting overspending and made branch-level performance comparisons nearly impossible.

This project delivers an interactive Power BI Intelligence Dashboard that provides a real-time, self-service view of actual expenses vs. budget. It empowers management to perform deep-dive analysis by branch, expense category, and time periods, facilitating faster and more accurate financial decision-making.


🏗️ **Data Architecture & Modeling**
The project utilizes a Star Schema to ensure optimal performance and scalability:
_Fact Table:_ Expense Transactions (Centralized spending data).
_Dimension Tables_: Branch, Expense Category, and Date.
_Data Preparation:_ Cleaned and transformed raw data using Power Query (M-Language) to ensure data integrity and consistency.


📈 **KPIs & Analytical Metrics**
The dashboard focuses on core financial health indicators:

Actual vs. Budget: Real-time monitoring of spending against financial targets.

Remaining Budget: Instant visibility into available funds.

Variance Analysis: Identification of fiscal deviations.

Trend Tracking: Monthly and YTD cumulative spend analysis.


**Advanced DAX Implementation**

Budget Variance % = DIVIDE([Actual Expenditure] - [Budget], [Budget])

Remaining Budget = [Budget] - [Actual Expenditure]

% Budget Used = DIVIDE([Actual Expenditure], [Budget])


🖥️ **Dashboard Features**
Granular Filtering: Dynamic slicers for Expense Category, Branch, and Year.

Time-Intelligence Toggles: Switch between Quarterly and Monthly views seamlessly.

Interactive Drill-downs: Click-to-filter capabilities to explore data from a macro level down to specific branch behaviors.


💡 **Actionable Business Insights**
The data revealed critical patterns that led to strategic adjustments:

Cost Concentration: Rent was identified as the largest driver at 15.67 billion (~68% of total), prompting an immediate lease renegotiation review.

Accountability: The Head Office accounted for 14.5 billion, leading to a detailed cost audit for HO operations.

Seasonality: Identified a recurring spend peak in January, allowing for better cash flow forecasting and budget front-loading in subsequent years.


🚀 **Impact & Deliverables**
Operational Efficiency: Automated the entire reporting pipeline, reducing manual reporting turnaround time.

Data Accuracy: Eliminated manual entry risks through a streamlined, automated data workflow.

Strategic Self-Service: Empowered branch managers to monitor their own "Remaining Budget" in real-time, fostering a culture of financial accountability.

📂 **How to Access**

Download the .pbix file from this repository.

Open with Power BI Desktop.
