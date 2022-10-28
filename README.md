# energy-perspective-2025-2030-germany


Description
-----------

This is a repository containing a scenario that implements the market for energy in Germany and its projections 2025, 2030 report for:

* electricity, 
* heat. 

It is meant to be used in `premise` in addition to a global IAM scenario, to provide 
refined projections at the country level.

This data package contains all the files necessary for `premise` to implement
this scenario and create market-specific composition for electricity and heat.

Sourced from publication
------------------------

....

Data validation 
---------------

[![goodtables.io](https://goodtables.io/badge/github/premise-community-scenarios/energy-perspective-2050-switzerland.svg)](https://goodtables.io/github/premise-community-scenarios/energy-perspective-2050-switzerland)

Test 
----

![example workflow](https://github.com/premise-community-scenarios/energy-perspective-2050-switzerland/actions/workflows/main.yml/badge.svg?branch=main)

Ecoinvent database compatibility
--------------------------------

ecoinvent 3.8 cut-off

IAM scenario compatibility
---------------------------

Among the following coupling between IAM and DE_20_25_30 scenarios, we selected REMIND SSP2-Npi:

| IAM scenario           | DE_20_25_30 scenario     |
|------------------------|----------------------|
| IMAGE SSP2-Base        | Business As Usual    |
| IMAGE SSP2-RCP26       | ZERO Basis (default) |
| IMAGE SSP2-RCP26       | ZERO A               |
| IMAGE SSP2-RCP26       | ZERO B               |
| IMAGE SSP2-RCP26       | ZERO C               |
| IMAGE SSP2-RCP19       | ZERO Basis (default) |
| IMAGE SSP2-RCP19       | ZERO A               |
| IMAGE SSP2-RCP19       | ZERO B               |
| IMAGE SSP2-RCP19       | ZERO C               |
| REMIND SSP2-Base       | Business As Usual    |
| REMIND SSP2-PkBudg1150 | ZERO Basis (default) |
| REMIND SSP2-PkBudg1150 | ZERO A               |
| REMIND SSP2-PkBudg1150 | ZERO B               |
| REMIND SSP2-PkBudg1150 | ZERO C               |
| REMIND SSP2-PkBudg500  | ZERO Basis           |
| REMIND SSP2-PkBudg500  | ZERO A               |
| REMIND SSP2-PkBudg500  | ZERO B               |
| REMIND SSP2-PkBudg500  | ZERO C               |



Electricity
***********

* `market for electricity, high voltage, DE_20_25_30` (DE)
* `market for electricity, medium voltage, DE_20_25_30` (DE)
* `market for electricity, medium voltage, DE_20_25_30` (DE)

These markets are relinked to activities that consume electricity in Germany.


That market itself relies on imports from the rest of Europe, which is
provided by the regional IAM market for European electricity (blue boundaries in map above).



Heat
*************

* `market for process heat, DE_20_25_30` (DE)

For hydrogen, this includes the domestic and foreign production, via electrolysis.
The latter is produced in the neighboring countries, using
the corresponding markets for electricity.

For compressed gas, this includes the provision of natural gas, biomethane
and synthetic gas (from neighboring countries), using
the corresponding markets for electricity.

How are technologies mapped?
---------------------------
The tables below show how the mapping between reported technologies
and LCI datasets is done. Unless specified otherwise, ecoinvent
LCI datasets are used.

Electricity
***********

| Technologies in EP2050+            | LCI datasets used                                               | Remarks                                                                                                                   |
| ---------------------------------- | --------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| Hydro, run-of-river                | electricity production, hydro, run-of-river                     |
| Hydro, alpine reservoir            | electricity production, hydro, reservoir, alpine region         |
| Nuclear, Boiling water reactor     | electricity production, nuclear, boiling water reactor          | The split between boiling water and pressure water is not provided. We use the current split, based on production volume. |
| Nuclear, Pressure water reactor    | electricity production, nuclear, pressure water reactor         |
| Conventional, Waste-to-Energy      | treatment of municipal solid waste, incineration                |
| Conventional, Other                | electricity production, natural gas, combined cycle power plant | The report does not specify what "Other" is. Assumed to be natural gas.                                                   |
| Conventional, Coal                 | electricity production, hard coal                               |
| Conventional, Natural gas          | electricity production, natural gas, combined cycle power plant |
| Renewable, Photovoltaic            | electricity production, photovoltaic                            | Datasets from 10.13140/RG.2.2.17977.19041.                                                                                |
| Renewable, Wind turbines, Onshore  | electricity production, wind, 1-3MW turbine, onshore            |
| Renewable, Wind turbines, Offshore | electricity production, wind, 1-3MW turbine, offshore           |
| Renewable, Geothermal              | electricity production, deep geothermal                         | Dataset provided by premise, based on the geothermal heat dataset of ecoinvent.                                           |
| Renewable, Biomass                 | heat and power co-generation, wood chips, 6667 kW               |
| Renewable, Biogas                  | heat and power co-generation, biogas, gas engine                |

Liquid fuels
************


| Technologies in EP2050+            | LCI datasets used                                               | Remarks                                                                                                             |
|------------------------------------|-----------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| Gas                             | heat production, natural gas, at industrial furnace >100kW                                   | 
| Oil                         | heat production, light fuel oil, at industrial furnace 1MW         
| Coal                  | heat production, at hard coal industrial furnace 1-10MW |
| Electricity                             | market for electricity  |  generated by premise in this project                               |
| CHP                         | ethanol production from sugar beet                |
| Biomass                   | heat and power co-generation, wood chips, 6667 kW, state-of-the-art 2014 |  
| Hydrogen                  | heat production, biomethane, at boiler condensing modulating <100kW, modified with hydrogen | generated  by premise. |                                                                                              |




Gaseous fuels
*************


| Technologies in EP2050+ | LCI datasets used                                                                                                                      | Remarks          |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------|------------------|
                                                                         | Provided by premise. |
| Hydrogen, imported      | hydrogen production, electrolysis, 25 bar, imported, EP2050                                                                            | Provided by premise. |




How to use it?
--------------

```python

    import brightway2 as bw
    from premise import NewDatabase
    from datapackage import Package
    
    
    fp = r"https://github.com/Conlod/energy-perspective-2050-switzerland/blob/main/datapackage.json"
    heat = Package(fp)
    
    bw.projects.set_current("your_bw_project")
    
    ndb = NewDatabase(
            scenarios = [
                {"model":"remind", "pathway":"SSP2-Npi", "year":2020,},
                {"model":"remind", "pathway":"SSP2-Npi", "year":2025,},
                {"model":"remind", "pathway":"SSP2-Npi", "year":2030,},
            ],        
            source_db="ei_3.8_cutoff",
            source_version="3.8",
            key= 'tUePmX_S5B8ieZkkM7WUU2CnO8SmShwmAeWK9x2rTFo=',
            external_scenarios=[
                heat, # <-- list datapackage objects here
            ] 
        )
```

