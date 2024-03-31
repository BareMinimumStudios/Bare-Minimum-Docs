---
title: Datapack Setup
project: data-attributes
---

!!! abstract ""
    This section provides an overview of the datapack structure used by Data Attributes for Minecraft 1.20.1.

- ### Structure

Data Attributes works using Minecraft’s data resource system. The file hierarchy for this is `data/namespace/attributes`. 

Where `namespace` is either the default `minecraft` or any given modid. 

By default, nothing exists in this directory, as all json data is optional. However, a fully populated directiory contains the files `entity_types.json`, `functions.json`, `properties.json` and `/overrides/` (which is a folder). 

This takes the form shown below:

 ![datapack structure example](../../assets/data-attributes/file-structure.png)