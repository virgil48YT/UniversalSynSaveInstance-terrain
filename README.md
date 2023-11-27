# Loadstring

```lua
local Params = {
	RepoURL = "https://raw.githubusercontent.com/luau/SynSaveInstance/main/",
	SSI = "saveinstance",
}
local synsaveinstance = loadstring(game:HttpGet(Params.RepoURL .. Params.SSI .. ".luau", true), Params.SSI)()
```

# Syn Save Instance

Or shortly SSI, a project aimed at resurrecting saveinstance function from [Synapse X Source 2019].<br />
Reason: Many Executors fail miserably at providing good user experience when it comes to tinkering with saving instances

# TO-DOs:

- [ ] !!! Custom fallback Decompiler for ModuleScripts using require and then iterating through it, gathering all info about functions using [getupvals/getprotos/getconsts][debug], converting all DataTypes using tostring or Descriptors, and then perhaps converting to JSON. (Make use of op-codes from Dex?) !!!
- [ ] Check out varios Leaked Executors (Especially their Init / Lua scripts) to expand knowledge on the whole subject of saveinstance
- [ ] Look into adding support for Binary Format Output (rbxl/rbxm)
  - .RBXL files are similar to .RBXLX files but are saved in Binary format, which helps reduce the file size.
  - ! Check out [Rojo Rbx Dom Binary] & [Roblox Format Specifications Binary] for more documentation about the Binary File Format!
  - ! Also see [buffer], [bit32] libraries as well as [pack]/[unpack] from the [string] library for more information on how you can implement something like this!
- [x] Add `continue` where needed
- [ ] Benchmark `next, ` and numerical loops on both lua & luau then decide which and where to use
- [ ] Add Documentation similar to [KRNL Docs] or [Synapse X Docs] / [Synapse X Docs Old]
- [x] ~~Add fallback function for appendfile (whether through storing current xml as string or with use of readfile)~~ Removed Appendfile entirely
- [x] Add getproperties as fallback for specialinfo
- [ ] Add Redirects to some special (in a bad way 😡) values, more info @ ~~[PropertyPatches v1]~~ [PropertyPatches v2]+[PropertyPatches v3], otherwise they will fallback to default when file is opened
  - Not all though, test each & see if it carries over or not (when file is opened)..
  - All current redirects: [Here](https://github.com/luau/SynSaveInstance/blob/main/TODO/PropertyPatches)
- [ ] Add more Fixes for Errors that **_can_** pop up during opening process
- [x] Add Optional tags support
- [ ] Add readbinarystring or readbinarystringpropertyvalue/readbspval (elysian) as fallback for gethiddenproperty
- [ ] Add table.clone instead {} in some cases if possible
- [ ] Avoid scanning for default values of properties if those properties won't get serialized anyway (e.g. don't have a Descriptor)
- [x] Add --!native tag just in case
- [ ] Find default values of binarystring properties (MaximumADHD might have a clue)
  - LOOK INTO Instance:IsPropertyModified & Instance:ResetPropertyToDefault
- [x] ~~Auto-Detect DataTypes/ValueType Categories of Properties (CFrame, UDim2 so on)~~ Full API Dump Solves this ?
- [x] Bring said DataType serializer into an outside function
- [ ] ~~Bypass NotCreatable by hardcoding links/references/indexes to said Classes~~ Should be Solved by IsPropertyModified
  - Example: Terrain class can be indexed by doing workspace.Terrain but is NotCreatable
- [x] Check if table.concat is actually the fastest way as compared to other alternatives (IT'S NOT)
- [x] Do ~~clean-up in inheritor &~~ (API Dumps solve this, illogical) automatically assume the top-most class that owns the property, while also cleaning up said property from classes that inherit from it
  - This will be only be needed if we try to implement our own scanning for hidden properties in which case a lot of duplicates might arise that need to be tracked down to instance they all inherit from & cleaned up respectively
- [x] Fix indexes being mixed up after table.remove shifting
- [x] Hidden properties
  - [x] ~~Scan for them~~ Full API Dump Solves this
  - [x] ~~Scan game & map instances in format {ClassName = {Instance1, Instance2} }, if none found then attempt to create proper Replica for them~~ Full API Dump Solves this
  * This will help with getting many ValueTypes accurately, especially BinaryStrings vs strings
  - [x] ~~Inherit them properly & do the clean-up~~ Full API Dump Solves this
  - [x] ~~Tell whether ValueType is string or BinaryString~~ Full API Dump Solves this
- [ ] Support for Model files:
  - [x] rbxmx (xml)
  - [ ] rbxm (binary)
- [ ] ~~Possibly convert to non-Name tables & use instance references instead (Perhaps make a config Bool Toggle for this, false by default), ex. DecompileIgnore = {game.CoreGui}~~ Add too much complexity for now
  - This will allow for more flexibility of saveinstancing
- [x] ~~Remove Useless tables & functions of specialinfo~~ Repurposed
- [x] Implement [Luau Syntax] (matters for performance!):

  - [x] Compound Operators
  - [x] Avoid using `next`, `ipairs` & `pairs`
  - [ ] Type-checking (😩🙀)
  - [ ] `local maxValue = if a > b then a else b` expressions
  - [ ] print(`Bob has {count} apple(s)!`) expressions
  - [ ] Floor division

- [ ] Speed things up as much as possible
  - Requires benchmarks
  - Requires looking at other scripts of ours that are aimed at speed & performance
- [x] Support for NotScriptable Properties
  - Requires gethiddenproperty support
- [ ] Support for as many [KRNL-like saveinstance Options]:
  - Change mode to invalid mode like "custom" if you only want to save ExtraInstances
  * [x] Decompile (! This takes priority over OPTIONS.noscripts if set !)
  * [x] DecompileIgnore
  * [x] DecompileTimeout (! This takes priority over OPTIONS.timeout if set !)
  * [x] ExtraInstances
  * [x] FilePath
  * [x] IgnoreDefaultProps
  * [x] IsolateStarterPlayer
  * [x] NilInstances
  * [x] Object (for .rbxmx files)
  * [x] RemovePlayerCharacters
  * [x] SavePlayers
  * [x] ShowStatus
  * [ ] IsolatePlayerGui
- [ ] Support for as many Executors as possible (🤢🤮)
- [x] ~~Use getspecialinfo fallback function carefully as it's hardcoded~~ Useless because there's no way to tell if the Property Values of those instances are default or not
  - LOOK INTO Instance:IsPropertyModified & Instance:ResetPropertyToDefault
- [x] Isolators must clear
- [x] Store all functions outside that are used during saveinstancing for sake of performance
- [x] ~~Remove buffersize, savebuffer & so on for sake of performance by concatenating <Item> strings to total string then writing it to file (no extra steps like table.concat)~~ table.concat proved faster in the case of huge amount of concatenations
  - Test table.concat vs string ..= with a full buffer (this benchmark differs per usecase)
- [ ] Make sure BinaryStrings are compared to Defaults properly (aka in same format)
- [ ] Add Option to restart saveinstance from the point that it crashed on
- [ ] Check out [DataType Exceptions]
- [x] Add README Similar to current Synapse
- [ ] Ignore all properties of instances that aren't Local or Module Scripts except Name if mode is set to "scripts"
- [ ] Maybe modes should do more than just affecting the list of instances to save, like changing IgnoreDefaultProperties to false if mode is "full" for example
- [x] Add Support for [SharedStrings]
  - Fun fact: SharedStrings can also be used for ValueTypes that aren't `SharedString`, this behavior is not documented anywhere but makes sense (Could create issues though, due to _potential_ ValueType mix-up). By replacing `<BinaryString name="Tags">Base64EncodedValue</BinaryString>` with `<SharedString name="Tags">UniqueIdentifierForSharedString</SharedString>` & putting `<SharedString md5="UniqueIdentifierForSharedString">Base64EncodedValue</SharedString>` into SharedStrings container you can achieve this amazing behaviour. This should be only enabled using an optional setting<br />Only known to work with (probably because both are base64 encoded):
  * BinaryString
- [x] Add Lua & Luau versions instead of merged (WARNING: LUAU WILL ALWAYS BE MORE UPDATED THAN LUA VERSION - IDC & CBA SIMPLY, lua version exists just for sake of old & bad executors, ask devs of your executors to support luau as its latest & greatest)
- [ ] Add Support for multiple Instances to be saved as a model
- [ ] Do something about devs renaming Services therefore bypassing Ignore lists (CoreGui/CorePackages are not affected)
  - LOOK INTO Instance:IsPropertyModified & Instance:ResetPropertyToDefault
- [ ] Fix Player's Characters not being visible (must Refresh MeshId)
  - "https://assetdelivery.roblox.com/v1/asset/?id=" Could cause issues too (needs testing)
  - Perhaps add a possible FIX script to README
- [ ] Be able to exclude / blacklist any mentions of certain string in other strings
  - Example: You wish to blacklist your player's name from appearing in any property value
  - Default options like IsolateSomething might also use / influence this
- [ ] Force disable ParticleEmitters in case something like IgnorePropertiesOfNotScriptsOnScriptsMode is enabled (they stack in one place and create huge lag)
- [ ] Be able to specify which special properties you want saved (to avoid saving all)

# Acknowledgments

This document is based largely on the efforts of [@Anaminus] & [@Dekkonot], authors of the [Roblox Format Specifications]. Additional
resources include:

- [Syngp Synapse X Source code 2019][Synapse X Source 2019] for base saveinstance code (extended by [@mblouka] & [@Acrillis])
- [Rojo Rbx Dom Xml] for being a fallback documentation in case something wasn't clear in the [Roblox Format Specifications]
- [Roblox File Format] for a list of redirects of old/deprecated xml properties that still use the old tag values
- [Roblox Client Tracker] for an extended & close to full JSON Api Dump (with hidden properties & default values)

*** View source code of this file for more credits

[@Acrillis]: https://github.com/Acrillis
[@Anaminus]: https://github.com/Anaminus
[@Dekkonot]: https://github.com/Dekkonot
[@mblouka]: https://github.com/mblouka
[bit32]: https://create.roblox.com/docs/reference/engine/libraries/bit32
[buffer]: https://create.roblox.com/docs/reference/engine/libraries/buffer
[pack]: https://create.roblox.com/docs/reference/engine/libraries/string#pack
[unpack]: https://create.roblox.com/docs/reference/engine/libraries/string#unpack
[string]: https://create.roblox.com/docs/reference/engine/libraries/string
[DataType Exceptions]: https://github.com/rojo-rbx/rbx-dom/blob/master/rbx_reflector/src/cli/generate.rs#L260
[KRNL Docs]: https://app.archbee.com/public/PREVIEW-2Jp4SDaAD4P1COFfx1p_t/PREVIEW-EtjA4sQe5zYUxIHwA6CqJ#mDB9D
[KRNL-like saveinstance Options]: https://app.archbee.com/public/PREVIEW-2Jp4SDaAD4P1COFfx1p_t/PREVIEW-EtjA4sQe5zYUxIHwA6CqJ#mDB9D
[Rojo Rbx Dom Xml]: https://github.com/rojo-rbx/rbx-dom/blob/e9732e427b8f0903b6ec9f5d02aa3f1f9e884e94/docs/xml.md
[Rojo Rbx Dom Binary]: https://github.com/rojo-rbx/rbx-dom/blob/e9732e427b8f0903b6ec9f5d02aa3f1f9e884e94/docs/binary.md
[Luau Syntax]: https://luau-lang.org/syntax
[Roblox Client Tracker]: https://github.com/MaximumADHD/Roblox-Client-Tracker
[Roblox File Format]: https://github.com/MaximumADHD/Roblox-File-Format
[Roblox Format Specifications]: https://github.com/RobloxAPI/spec/
[Roblox Format Specifications Binary]: https://github.com/RobloxAPI/spec/blob/master/formats/rbxl.md
[SharedStrings]: https://github.com/RobloxAPI/spec/blob/master/formats/rbxlx.md#sharedstring
[Synapse X Docs Old]: https://synapsexdocs.github.io/custom-lua-functions/misc-functions/#save-instance
[debug]: https://web.archive.org/web/20221021015553/https://docs.synapse.to/reference/debug_lib.html
[Synapse X Docs]: https://web.archive.org/web/20230318113846/https://docs.synapse.to/reference/misc.html
[Synapse X Source 2019]: https://github.com/Acrillis/SynapseX
[PropertyPatches v1]: https://github.com/MaximumADHD/Roblox-File-Format/blob/main/Plugins/GenerateApiDump/PropertyPatches.lua#L72
[PropertyPatches v2]: https://github.com/rojo-rbx/rbx-dom/tree/master/patches
[PropertyPatches v3]: https://github.com/rojo-rbx/rbx-dom/blob/9716af360307eb5da5f97d54d84d694b2bc06acf/rbx_dom_lua/src/customProperties.lua
