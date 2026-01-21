# Demo Title: Forecasting Model Comparison Framework

==========================================================

Multi-model forecasting system with automated validation and selection logic. Compares regression, rule-based, and baseline approaches using backtested MAPE evaluation. Built with Python and Power BI

==========================================================

Purpose:
This framework demonstrates a systematic approach to model selection by comparing three forecasting methodologies across multiple business dimensions. Rather than defaulting to a single forecasting technique, the system evaluates model performance at the State and ProductType level using backtested validation metrics (MAPE/RMSE), then recommends the optimal model for each segment.

Forecasting Methodologies (3):
  1. Regression
  2. Rules-based
  3. Simple Moving Average (SMA)

Dimensions (4):
  1. Product Type (4)
  2. States (50)
  3. Months (12)
  4. Years (2020-2023)

Forecasted 2024 MAPE Evaluation vs Actual 2024:
  1. Regression MAPE (REG%Δ) = Absolute% difference between Regression Forecast and Actual
  2. Rules-Based MAPE (RBD%Δ) = Absolute% difference between Rules-based Forecast and Actual
  3. SMA MAPE (SMA%Δ) = Absolute% difference between SMA Forecast and Actual

MAPE Result:
  Recommends the optimal model for each segment based on 2024 MAPE Evaluation vs Actual 2024 and trains on updated dimension Year (2020-2024) to produce forecast for 2025

The project showcases end-to-end thinking: Python handles model development and validation using proper train/test splits, while Power BI operationalizes the results through an interactive interface that allows business users to explore forecasts and understand model performance trade-offs across different scenarios.

==========================================================

Forecasting Models (In Depth)

==========================================================

1. Regression Model: Trained on Years (2020-2023)   
    Train on 2020-2023→Generate 2024 forecasts→Calculate MAPE by State+Product
    12 months × 4 years(2020-2023) × 50 states x 4 ProductTypes= 9,600 obs
    X matrix: 9,600 obs × 65 columns (1 intercept + 11 months + 1 year + 49 states + 3 ProductType dummies)
    9,600 ÷ 65 = 147.7 observations per coefficient

    Intercept (β₀):
    - β₀ = Baseline (December 2020, baseline State, baseline ProductType)

    Month Coefficient (β_Month):
    - β_Month = Isolated effect of Selected Month (Jan-Nov; Dec is baseline)

    Year Coefficient (β_Year):
    - β_Year = Annual growth rate per year since 2020

    State Coefficient (β_State):
    - β_State = Isolated effect of Selected State (49 states; one baseline)

    ProductType Coefficient (β_ProductType):
    - β_ProductType = Isolated effect of Selected ProductType (3 types; one baseline)

    Years Since 2020 (YS):
    - YS = 2024 - 2020 = 4

    Regression Forecast 2024 (REG_f2024):
    - REG_f2024 = β₀ + β_Month + (β_Year × YS) + β_State + β_ProductType

3. Rules-Based: Trained on Years (2020-2023)

    Compound Annual Growth Rate (CAGR_20-23):
    - CAGR_20-23 = 1 + CAGR(2020-2023)

    Actual Sales 2023 (AS_2023):
    - AS_2023 = Actual Sales for Selected[Month 2023, State, ProductType]

    Month Delta(MΔ):
    - MΔ = Selected Month Avg Sales - Average of all months

    State Delta (SΔ):
    - SΔ = Avg Monthly Sales Selected State - Avg Monthly Sales All States

    ProductType Delta (PΔ):
    - PΔ = Avg Monthly Sales Selected Product Type  - Avg Monthly Sales All Product Types

    Rules-Based Forecast 2024 (RBD_f2024) = (AS_2023 + MΔ + SΔ + PΔ) × CAGR_20-23

4. SMA: Trained on Years (2020-2023)

    SMA Forecast 2024 (SMA_f2024):
    - SMA_f2024 = AVG(Selected Month [Jan,Feb..Dec][2020-2023]) for Selected State and ProductType

==========================================================

Forecasted 2024 MAPE Evaluation vs Actual 2024 (In Depth)

==========================================================
  
Actual Sales 2024 (AS_2024):
- AS_2024 = Actual Sales for Selected Month 2024, State, and ProductType

Regression MAPE (REG%Δ):
- REG%Δ = ABS(REG_f2024-AS_2024)/AS_2024

Rules-Based MAPE (RBD%Δ):
- RBD%Δ = ABS(RBD_f2024-AS_2024)/AS_2024

SMA MAPE (SMA%Δ):
- SMA%Δ = ABS(SMA_f2024-AS_2024)/AS_2024

Optimal Model Result (OMR):
- OMR = Model with Lowest MAPE

==========================================================

2025 Forecast Generation

==========================================================

After identifying the optimal model per segment using 2024 validation:

1. Regression Model: Retrained on Years (2020-2024)
   - 12,000 observations (12 months × 5 years × 50 states × 4 ProductTypes)
   - Generates forecasts for Jan-Dec 2025

2. Rules-Based: Retrained on Years (2020-2024)
   - AS_2024 replaces AS_2023
   - Deltas recalculated using 2020-2024 averages
   - CAGR recalculated across 2020-2024

3. SMA: Retrained on Years (2020-2024)
   - SMA Forecast 2025 = AVG(SM[2021, 2022, 2023, 2024])
   - Maintains 4-year rolling window

4. Power BI Implementation:
   - Displays all three model forecasts for 2025
   - Shows 2024 MAPE by segment for model comparison
   - Recommends optimal model based on segment-specific validation performance
