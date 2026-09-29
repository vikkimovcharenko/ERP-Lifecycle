# 03. Bill of Materials (BOM) & Routing

## 1. Business Context
To transition from procurement to manufacturing, the system must understand how individual procured components (SKUs) are assembled into finished goods. The Bill of Materials (BOM) acts as the central recipe, linking the Master Item records to specific production routing steps.

## 2. Key User Stories
*   **US-BOM-01:** As a Production Engineer, I want to link multiple procured SKUs into a single Bill of Materials, so that the system knows exactly which parts to consume from inventory during manufacturing.
*   **US-BOM-02:** As the System, I want to validate that all required SKUs in the BOM are actively available in the Production Available (PA) inventory before allowing a Manufacturing Work Order to start.

