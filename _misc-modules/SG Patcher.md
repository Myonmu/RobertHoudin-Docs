---
layout: default
permalink: /misc-modules/sg-patcher
title: SG Patcher
---
# SG Patcher

SG Patcher is short for "**S**hader **G**raph Patcher".  The module directly manipulates `.shadergraph` and `.shadersubgraph`'s json content to add new shader properties to existing shader graphs and sub graphs. This is useful when creating modular shader functionalities.

## How the SG Patcher helps

Traditionally, if you have made a modular shader function, to add it to another shader, you would do the following:

1. Make the modular shader function a sub graph, either by using the "convert to sub graph" provided by Unity (which might not work), or manually create a sub graph and copy-paste all the nodes, create input properties, and finally create output properties.
2. In the shader that you wish to insert the sub graph, for all properties you wish to expose in the inspector, either copy-paste them from the sub graph or create them manually.
3. Connect the properties and output from other nodes to the sub graph.

This means you would always need to keep a template shader graph somewhere, which might or might not have unrelated test properties, and you have to remember where they are and what are required. You also need to open at least 2 shader graphs in this process, and if your graph is complex, it can be heavy for the editor.

SG Patcher simplifies "copy-paste properties" process by storing the properties in a dedicated scriptable object: instead of creating a template shader graph from which you can copy from, create an `SGPropertyPatchPreset` and add properties in the list. After that, you only need to assign the shader graph you wish to modify and click **Patch Shader Graph** or **Patch Sub Graph** in the context menu and the preset will append the properties to your shader.

## SG Property Patch Preset

`SGPropertyPatchPreset` is a scriptable object that stores shader properties - much like how it is done in shader graph's black board. 

> To create a `SGPropertyPatchPreset`: **Create/RobertHoudin/SG Property Patch Preset**.

![alt text{caption=SG Property Patch Preset in Inspector}](doc-res/misc-modules/SGPropertyPatchPreset-Inspector.png)

To patch a shader graph or sub graph:

1. Select or Drag and Drop a shader graph in **Sg To Patch** field, or a sub graph in **Sub Graph To Patch** field.
2. click on the three little dots just above the open button and select **Patch Shader Graph** or **Patch Sub Graph**.

![alt text{caption: Patching a shader graph}](doc-res/misc-modules/SGPropertyPatchPreset-Patching.png)

The properties you created in **Entries** will be written to the shader graph or sub graph.

{: .caveat }
> Currently, the patcher only **Appends** properties, never overwrite an existing property with a same **name**: if you already have a property named "Some Property" in a shader graph, and also "Some Property" in a patch preset, the property already in the shader graph will not be changed.

{: .caveat}
> The patcher doesn't ensure proper formatting of the appended data, but that doesn't affect Unity deserializing the json. This artifact can be simply fixed by opening and saving the shader graph in Unity. 


### Under the Hood

The patcher doesn't deserialize the json content of a shader graph, instead, it searches for the `m_Name` field. if the patcher finds a match then the property is skipped. 

When the name is not present in the shader graph, then the patcher constructs a `SGPropertyJsonBlock` which mimics shader graph's internal class - they would produce identical json serialization results. This block is then serialized and is appended directly at the end of the file. 

To make these patched properties visible to shader graph, there are two extra fields `m_ChildObjects` and `m_Properties` that must also be appended. The patcher uses regex to search for the fields and append a serialized `SGObjectReference` which is simply a wrapper of a property's *object id* for each property patched.