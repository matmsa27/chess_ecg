# CHANGELOG FOR BACKUP ZIP_FILES

ICU - v0.7 Stable version 03-05-2020
----------
 * Running analysis for the all models, including the MonitorMulparametric Model(integrated)
 * Is missing: 
    * Running analysis with redundance
    * Add sterotype for battery and running analysis


ICU - v0.6 Stable version 25-04-2020
----------
 * ErrorModelBehavior modelling to ManageBattery, but didn't work for the running the analysis
 * Running analysis for the models, but deem server did not behave well
 * Is missing: 
    * Get better results running Analyses
    * Test the analysis define DEEM Server parameters


ICU - v0.5 Unstable version 19-04-2020
----------
 * Remove CPU from each modelling
 * Create a new Interface for Voltage representing 5V inside the modelling
 * Add energy on sensor
 * Remove NoCardiacAnomalyDetected from DetectAomalies
 * Create a CentralProcess to receive OximeterPulse, Termometer and RateRespiratory config and Signals
 * Change name ElectricSignalDC interface to ElectricSignalAC
 * Change name Voltage intarface to VoltageDC
 * Is missing: 
    * ErrorModelBehavior modelling to ManageBattery
    * Running Analyses
    * To refine DEEM Server parameters

ICU - v0.4 Unstable version 12-04-2020
----------
 * Make improvements on general modelling and finish the general modelling


ICU - v0.3 Unstable version 08-04-2020
----------
 * Finish Link general modelling
 * Finish Composite for ServerResults
 * Is missing generate instances and Running Analysis


ICU - v0.2 Unstable version 07-04-2020
----------
 * Link alarms to all dispostives(ecg, termometer, pulse oximeter and rate respiratory)
 * Link pre configurations to dispositives models
 * Finish the class diagram to server results
 * Is missing general linking
 * Is missing composite for ServerResults

ICU - v0.1 Unstable version 07-04-2020
----------
 * Refactor all the project
 * Is missing the general integrations
 * Link pre cofinguration with models (ecg, term, oximeter and resp)
 * Is missing finish the server results with alarms and signals provided another models
 * Is missing configuration the analysis and running


v0.13 Stable version 30-03-2020
----------
 * Create a new class diagram to Integrate 4 modelling(battery, screen results, detect diseases and ecg)
 * Create a new compose for integrate 4 modelling(battery, screen results, detect diseases and ecg)
 * Running analysis for integrated modelling, works perfectly

v0.12 Stable version 24-03-2020
----------
 * Modelling General ICU with Multiparametric Editor
 * Add more components to modelling Mulparametric Monitor
 * Running alanysis

v0.11 Stable version 15-03-2020
----------
 * Modelling General ICU Environment
 * Is missing component that represent each modelling

v0.10 Stable version 04-03-2020
----------
 * Modelling ECGSignalFlow
 * Running Analysis to ECGSignalFlow


v0.9 Stable version 04-03-2020
----------
 * Modelling ScreeResults
 * Running Analysis to ScreenResults
 * PS: I cannot runned yet the analysis for each modelling. I runned the analysis for each modelling time by time

v0.8 Stable version 04-03-2020
----------
 * Modelling AlarmBattery and DetecteDiseases in a new project
 * Analysis runned and tested for AlarmBattery, will running on DetecteDiseases yet

v0.7 Stable version 27-02-2020
----------
 * Create another project only with SystemView, contains modelling to AlarmBattery, Screen Results and ECG Signal Flow
 * Refactor SystemView and run analysis on SystemView


v0.6 UnStable version 13-02-2020
----------
 * Refactor models and organize the architecture models


v0.5 UnStable version 11-02-2020
----------
 * Organize DetectDieses on packages
 * Organize ECGSignalFlow on packages
 * Organize ScreenResults on packages
 * Organize GeneralECG on packages


v0.4 UnStable version 11-02-2020
----------
 * Organize BatteryAlarm on packages


v0.3 - Stable version 06-02-2020
----------
 * End modelling, is missing configure the analysis
