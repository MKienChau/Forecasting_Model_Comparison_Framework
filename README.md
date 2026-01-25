# Demo Title: Forecasting Model Comparison Framework

==========================================================

Multi-model forecasting system with automated validation and selection logic. Compares regression, rule-based, and baseline approaches using backtested MAPE evaluation. Built with Python and Power BI

==========================================================

Purpose:
This framework demonstrates a systematic approach to model selection by comparing three forecasting methodologies across multiple business dimensions. Rather than defaulting to a single forecasting technique, the system evaluates model performance at the State and Product(Golf Clubs) level using backtested validation metrics (MAPE/RMSE), then recommends the optimal model for each segment.

Forecasting Methodologies (3):

  1. Regression
  2. Rules-based
  3. Simple Moving Average (SMA)

Dimensions (4):

  1. Years (2020-2023)
  2. Clubs (4,Driver,Fairway,Hybrids,Irons)
  3. Top 26 Weeks by Netsales
  4. Top Markets (States (62, US=50, CA=12) must Qualify in at least 1 of the following)
    A. Sales + (Margin% gate=rank middle half+ in Margin% overall):
    - 1st of Division, Top Quartile of Region/Overall for Sales
    B. Volume + (Margin$ gate=rank middle half+ in Margin$ overall):
    - 1st of Division, Top Quartile of Region/Overall for Units Sold
    C. Profit + (Volume gate=rank middle half+ in Units overall)
    - 1st of Division, Top Quartile of Region/Overall for EITHER Margin$ or Margin%

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

2. Rules-Based: Trained on Years (2020-2023)
    Compound Annual Growth Rate (CAGR_20-23):
    - CAGR_20-23 = 1 + CAGR(2020-2023)

    Actual Sales 2023 (AS_2023):
    - AS_2023 = Selected Week's Actual sales in 2023

    Week Delta(MΔ):
    - MΔ = Selected Week's AvgSales(2020-2023) - AvgSales(2020-2023) of all Weeks

    State Delta (SΔ):
    - SΔ = AvgSales(2020-2023) all Weeks for Selected State -AvgSales(2020-2023) of all States

    Club Delta (PΔ):
    - PΔ = AvgSales(2020-2023) all Weeks for Selected Club  - AvgSales(2020-2023) of all Clubs

    Rules-Based Forecast 2024 (RBD_f2024):
    - RBD_f2024= (AS_2023 + MΔ + SΔ + PΔ)*CAGR_20-23

3. SMA: Trained on Years (2020-2023)

    SMA Forecast 2024 (SMA_f2024):
    - SMA_f2024 = Selected Week's AvgSales(2020-2023) for Selected State and ProductType

==========================================================

Forecasted 2024 MAPE Evaluation vs Actual 2024 (In Depth)

==========================================================

Actual Sales 2024 (AS_2024):

  - AS_2024 = Selected Week's Actual sales in 2024

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

2. Rules-Based: Retrained on Years (2020-2024)

3. SMA: Retrained on Years (2020-2024)

4. Power BI Implementation:
   - Displays all three model forecasts for 2025
   - Shows 2024 MAPE by segment for model comparison
   - Recommends optimal model based on segment-specific validation performance
