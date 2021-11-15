---
marp: true
theme: uncover
---
<style>
h2 {
  position: absolute;
  left: 80px;
  top: 80px;
  right: 80px;
  height: 70px;
  line-height: 70px;
}
</style>
<!-- _class: invert -->

# Improving TestStand Developer Experience

<div style="font-size:50%;"></br>Michał Bieńkowski</br>15.11.2021</br><b>Global LabVIEW Architects' Summit</b></div>

---
<!-- _header: Random Ramblings on DX -->

## What is DX?

</br></br>

> Developer experience describes the interactions and feelings that a developer has when working with a body of code in order to meet a specific objective. You can think of developer experience as the user experience specifically for programmers. 

<div style="font-size:50%;"></br><a href="https://www.redhat.com/architect/developer-experience"><u>Source</u></a></div>

---
<!-- _header: Random Ramblings on DX -->

## What makes DX good?

</br></br>

> Do not forget to make your developers happy, and keep them happy!

<div style="font-size:50%;"></br><a href="https://developerexperience.io/practices/good-developer-experience"><u>Source</u></a></div>

---
<!-- _header: Random Ramblings on DX -->
<!-- _class: invert -->

# `Equals(GoodDX, GoodIDE)`?

<!-- 
  - Language,
  - Community
  - IDE
-->

---
<!-- _header: Random Ramblings on DX -->

## What makes IDE good?

</br></br>

* Encourages code reuse and saves us from reinventing the wheel
* Promotes automation, do tedious and boring tasks for us, and lets us focus on important things
* Provides practical toolboxes, tools palettes, etc.
* Provides context-aware information
* Allows personalization
* Provides useful shortcuts like **Quick Drop**

---
<!-- _header: Random Ramblings on DX -->

## Why do we like **Quick Drop**?

</br>

* Always at hand
* Exposes a lot of properties and functions through the VI Server which we can use for automation
* Easy to extend
* Simple, practical, and allows many customizations - precisely those **I need**
* Has nice community

---
<!-- _header: Random Ramblings on DX -->

# What else would we like to have in our IDE to have a good DX?

---
<!-- _class: invert -->

# `*.tsenv`

---
<!-- _header: TestStand Environment -->

## TestStand Directory Structure

</br></br>

- [\<TestStand>](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsfundamentals/infotopics/directories_ts/) (%TestStand%)
  - `TestStandPath_TestStand`
- [\<TestStand Public>](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsfundamentals/infotopics/directories_ts_public/) (%TestStandPublic%)
  - `TestStandPath_Public`
- [\<TestStand Application Data>](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsfundamentals/infotopics/directories_ts_app_data/) (%TestStandAppData%)
  - `TestStandPath_CommonAppData`
- [\<TestStand Local Application Data>](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsfundamentals/infotopics/directories_ts_local_app_data/)
  - `TestStandPath_LocalAppData`

---
<!-- _header: TestStand Environment -->

## Side-by-Side TestStand System Configurations

</br>

* Before 2016:
  * Manual file management 🤮
  * Scripts for file management 😵
  * Separate VM per TestStand system configuration 😐

---
<!-- _header: TestStand Environment -->

## TestStand Environments (tsenv)

</br></br>

* [Introduced in TestStand 2016](https://zone.ni.com/reference/en-XX/help/370052AA-01/tshelp/infotopics/2016whatsnew/#Env)
* [Enables mltpl side-by-side configs on a sgl system](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsfundamentals/infotopics/tsenv/)
* Gives us better control over:
  * \<TestStand Public> ➔ `Public`
  * \<TestStand Application Data> ➔ `CommonAppData`
  * \<TestStand Local Application Data> ➔ `TestStandPath_LocalAppData`
* [Limitations](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsfundamentals/infotopics/tsenv_limitations/) (**User Management**, Remote Engine, Simultaneous Execution)

---
<!-- _header: TestStand Environment -->

## How to Create `*.tsenv`

</br>

* Select menu **Configure»Environment**
* Configure: ![Configure Environment](https://github.com/425J/GLA/blob/main/images/ConfigEnv.png?raw=true)
* Restart using `tsenv` 🙁

---
<!-- _header: TestStand Environment -->

## `425J.tsenv` Content

</br>

```
[TestStandPaths]
CommonAppData = "CommonAppData"
Public = "Public"
LocalAppData = "LocalAppData"
```

---
<!-- _header: TestStand Environment -->

## Side Notes (1/2)

</br></br>

* [`Engine.GetEnvironmentPath`](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsapiref/reftopics/engine_getenvironmentpath_m/)
* [`EngineInitializationSettings.SetEnvironmentPath`](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsapiref/reftopics/engineinitializationsettings_setenvironmentpath_m/)
  * [Example](https://forums.ni.com/t5/NI-TestStand/How-to-specify-environment-when-using-custom-OI/m-p/4184312#M64285)
* [How to disable environments in Windows registry](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsfundamentals/infotopics/tsenv_limitations/)
* There is no need to copy all files from `<TestStand>`
  * To add default tools, select menu **Tools»Customize** and then click **Export Items to File**. Created `*.ini` file move to `<Public>\Setup\ToolMenusToInstall`.

---
<!-- _header: TestStand Environment -->

## Side Notes (2/2)

</br>

* Adding the environment to the version control system gives us full control over the configuration
  * Keeping environment files in the project repo
  * Separate repo for environment

---
<!-- _header: TestStand Environment -->

## Choosing an Environment

</br></br>

* To launch Sequence Editor in an environment:
  * Use menu **Configure»Environment** 🥴
  * Set fixed environment using Windows registry 😬
    * [Example](https://forums.ni.com/t5/Discussions-au-sujet-des-autres/TestStand-2016-SP1-Lancement-de-TestStand-avec-un-quot/m-p/3788420#M52881)
  * Use CLI and `/env` switch 🥳
    * [TestStand executables supporting the `/env` switch](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsfundamentals/infotopics/tsenv_choosing/)

---
<!-- _header: TestStand Environment -->

## SeqEdit CLI

</br></br>

[Startup Options](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsfundamentals/infotopics/startup_opt/)

`"%TestStandBin%\SeqEdit.exe" /help`

[`ApplicationMgr.ProcessCommandLine`](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsuiref/reftopics/applicationmgr_processcommandline_p/)

</br>

There is no `/quiet` switch 😥

![bg right h:99%](https://github.com/425J/GLA/blob/main/images/CLI.png?raw=true)

---
<!-- _header: TestStand Environment -->

## Startup Script (1/2)

</br>

```bat
@ECHO OFF

SETLOCAL
SET "InitialWorkingDir=%~dp0"
CD "%InitialWorkingDir%"
ECHO Initial working directory:   %InitialWorkingDir%
SET "TSEvironment=%InitialWorkingDir%425J.tsenv"
ECHO TestStand environment file:  %TSEvironment%

REM Start TestStand Sequence Editor using TestStand environment file.
START "Start SeqEdit" "%TestStandBin%\SeqEdit.exe" /env "%TSEvironment%"
ECHO Starting SeqEdit.exe:        %TestStandBin%\[92mSeqEdit.exe[0m
ENDLOCAL
```

---
<!-- _header: TestStand Environment -->

## Startup Script (2/2)

</br>

![Startup Script](https://github.com/425J/GLA/blob/main/images/Startup.png?raw=true)
⬇⬇⬇
![Status](https://github.com/425J/GLA/blob/main/images/Status.png?raw=true)

---
<!-- _header: TestStand Environment -->

## Summary

</br>

TestStand Environment is a perfect foundation for DX improvement.

---
<!-- _class: invert -->

# `layout_*.bin`

---
<!-- _header: TestStand UI Configuration -->

![bg h:80%](https://github.com/425J/GLA/blob/main/images/SeqEditMessyLayout.png?raw=true)

---
<!-- _header: TestStand UI Configuration -->

😲 **Overwhelmed?** 🥴
![bg opacity h:80%](https://github.com/425J/GLA/blob/main/images/SeqEditMessyLayout.png?raw=true)

---
<!-- _header: TestStand UI Configuration -->

![bg h:80%](https://github.com/425J/GLA/blob/main/images/SeqEditSimpleLayout.png?raw=true)

---
<!-- _header: TestStand UI Configuration -->

🤩 **Better?** 🥳
![bg opacity h:80%](https://github.com/425J/GLA/blob/main/images/SeqEditSimpleLayout.png?raw=true)

---
<!-- _header: TestStand UI Configuration -->

## Toolbars and Menus (1/2)

</br></br>

* **View»Customize Toolbars/Menus**
  * Create custom toolbar
    * Expose normally hidden items 🤫
      * [`Edit\Edit Paths`](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsref/infotopics/db_edit_paths_in_files/)
      * Other?
  * Alter menus
  * Enable large icons
* It's not possible to create a toolbar with custom tools 😢
* No custom icons 🙁

---
<!-- _header: TestStand UI Configuration -->

## Toolbars and Menus (2/2)

</br></br>

* **View»Customize Toolbars/Menus**
  * Alter keyboard shortcuts
    * Useful:`View\Windows`, `View\Sequences`, `View\Variables`, `View\Steps`, `View\Step Settings`, `Edit\Edit`, `Edit\Specify Module`
    * Useless:`Edit\Surround Selection With` 🤨🤔
  * For mouseless editing, pair with Quick Drop and Templates 🤠

---
<!-- _header: TestStand UI Configuration -->

## Expression Editing Options

</br></br></br></br>

* Select **Expression»Right Click»[Options](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsref/infotopics/db_exp_edit_options/)**
  * Hotkeys
  * Font Size

![bg right h:99%](https://github.com/425J/GLA/blob/main/images/ExpressionEdit.png?raw=true)

---
<!-- _header: TestStand UI Configuration -->

## Step List Configuration

</br></br>

* Select **Step List»Right Click»Step List Configurations»[Edit Step List Configurations](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsref/infotopics/db_step_list_config/)**
  * Seperete configurations for edit and execution
  * Appearance
    * Icons, Fonts, Colors, Other
  * Custom [columns](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsref/infotopics/db_configuration_prop_columns/)
    * [`SeqViewColumnTypes`](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsuiref/reftopics/seqviewcolumn_type_p/)
    * [`SeqViewColumn.Expression`](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsuiref/reftopics/seqviewcolumn_expression_p/)

---
<!-- _header: TestStand UI Configuration -->

## Custom SeqView Columns (1/5)

![bg h:50%](https://github.com/425J/GLA/blob/main/images/AttributesExp.png?raw=true)

<!--
Step.AsPropertyObject.HasAttributes ? (
	Trim(
		SearchAndReplace(
			SearchAndReplace(
				SearchAndReplace(
					SearchAndReplace(
						SearchAndReplace(
							SearchAndReplace(
								SearchAndReplace(
									SearchAndReplace(
										SearchAndReplace(
											Step.AsPropertyObject.Attributes.GetXML(XMLOption_ExcludeFlags OR XMLOption_ExcludeVersionInfo, 0),
																					"<Prop Name='", ""),
																					"' Type='Attributes'>", ""),
																					"' Type='Obj'>", ""),
																					"' Type='Boolean'>", ""),
																					"' Type='Number'>", ""),
																					"' Type='String'>", ""),
																					"</Prop>", ""),
																					"<Value>", ""),
																					"</Value>", ""))
) : ""
-->

---
<!-- _header: TestStand UI Configuration -->

## Custom SeqView Columns (2/5)

![bg h:50%](https://github.com/425J/GLA/blob/main/images/Attributes.png?raw=true)

---
<!-- _header: TestStand UI Configuration -->

## Custom SeqView Columns (3/5)

</br></br>

Result recording column `Expression`:
```
RunState.Engine.StationOptions.DisableResults ?
	"Disabled"
:(RunState.Sequence.DisableResults && Step.ResultRecordingOption != 2) ?
	"Disabled"
:(Step.ResultRecordingOption == 0) ?
	"Disabled"
:"Enabled"
```

Result recording column `Text Color Expression`:
```
tsWhite
```

---
<!-- _header: TestStand UI Configuration -->

## Custom SeqView Columns (4/5)

</br></br>

Result recording column `Background Color Expression`:
```
RunState.Engine.StationOptions.DisableResults ?
	tsBlack
:(Step.ResultRecordingOption == 0) ?
	tsDarkRed
:(RunState.Sequence.DisableResults && Step.ResultRecordingOption == 1) ?
	tsDarkGray
:(RunState.Sequence.DisableResults && Step.ResultRecordingOption == 2) ?
	tsDarkBlue
:(Step.ResultRecordingOption == 1) ?
	tsDarkGreen
:tsBlue
```

---
<!-- _header: TestStand UI Configuration -->

## Custom SeqView Columns (5/5)

![bg h:50%](https://github.com/425J/GLA/blob/main/images/Reporting.png?raw=true)

---
<!-- _header: TestStand UI Configuration -->

## Sequence Editor Options

</br>

* [**Configure»Sequence Editor Options**](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsref/infotopics/db_seq_edit_opt/)
  * Disable "View User Manager" Command
  * Display Documents in Tabs
  * Lock UI
  * **Save Configuration**
  * Automatically Load Configuration at Startup
* `<CommonAppData>\Cfg\SeqEdit\layout_*.bin`

---
<!-- _header: TestStand UI Configuration -->

## [User-Specific Application Data](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsfundamentals/infotopics/directories_ts_local_app_data/)

</br></br>

* **layout_current.bin** — Layout of the sequence editor the last time it was closed, including window position, size, docked state, and other settings for the panes, menus, and toolbars. 
* **SeqEdit.xml** — Configuration options for the sequence editor, such as panel size and font settings used on the Insertion Palette pane and the Variables pane. See also [`ApplicationMgr.ConfigFile`](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsuiref/reftopics/applicationmgr_configfile_p/).

---
<!-- _header: TestStand UI Configuration - Advanced Layout Example 1 -->

![bg w:100%](https://github.com/425J/GLA/blob/main/images/SeqEdit1.png?raw=true)

---
<!-- _header: TestStand UI Configuration - Advanced Layout Example 2 -->

![bg w:100%](https://github.com/425J/GLA/blob/main/images/SeqEdit2.png?raw=true)

---
<!-- _header: TestStand UI Configuration - Simple Layout Example 1 -->

![bg w:100%](https://github.com/425J/GLA/blob/main/images/SeqEdit3.png?raw=true)

---
<!-- _header: TestStand UI Configuration - Simple Layout Example 2 -->

![bg w:100%](https://github.com/425J/GLA/blob/main/images/SeqEdit4.png?raw=true)

---
<!-- _header: TestStand UI Configuration -->

# [Layout Loader Repo](https://github.com/425J/LayoutLoader)

---
<!-- _header: TestStand UI Configuration -->

## Summary

</br>

Perhaps the SeqEdit does not offer many possibilities, but what we have at our disposal is usually enough.

<!-- 
  - If it's not enough then consider different tool
  - There are enough features for developers - more or less
  - There could be more features for making "operator layout"
    - We still can use on-the-fly reporting
    - Instrument Studio integration?
  - NI could take a closer look at SeqEdit customization and add more features
  - Bonuses
    - You don't need to build custom GUI
    - SeqEdit is faster
-->

---
<!-- _class: invert -->

# `Templates.ini`

---
<!-- _header: Templates -->

## Step Templates

</br>

* Can be used as clipboard 📋
* Can be used as toolbox/palette 🛠
  * Faster/easier to create and add then step types ⚡
  * In theory provides worse DX 🙄
    * Or maybe not? 🤔
      * Let's consider Sequence Adapter...

---
<!-- _header: Templates -->

## Example

</br></br>

<img src="images\CMD1.png" width="100%"/>
<img src="images\CMD2.png" width="100%"/>

<!-- 
  - The navigation is buggy - ⬆, ⬇ and ⬅ works, but not ➡ :(
-->

---
<!-- _header: Templates -->

## Driver Sequences

</br>

* `<Public>\Components\DriverSequences`
* Sequence File wrappers for PPLs
* Sequence File properties `No Model`
* `Parameters.Error`
* Automatically build driver templates 🤩

---
<!-- _header: Templates -->

# [Templates Generator Repo](https://github.com/425J/TemplatesGenerator)

---
<!-- _header: Templates-->

## Summary

</br>

Templates do not give the possibility to create a personalized editing panel, which means a potentially worse DX 😯, but on the other hand, if we always use the sequence adapter, the DX will be unified 😌. These templates are very quick to create and easy to use.

<!-- 
  - Simpler alternative for step types - but step type can add more customization in edit pane
  - Use CLI to auto start templates generation - useful if we have dynamic set of Driver Sequences
  - Can also generate PPL templates - slow execution of Module.LoadPrototype()
  - It is possible to set custom default step parameters
  - Mouseless navigation
    - The navigation is buggy - ⬆, ⬇ and ⬅ works, but not ➡ :(
-->

---
<!-- _class: invert -->

# TestStand **Quick Drop**

![bg right h:95%](https://github.com/425J/GLA/blob/main/images/TSQD.png?raw=true)

---
<!-- _header: TestStand **Quick Drop** -->

## TestStand **Quick Drop**

</br></br>

* [Introduced in TestStand 2019](https://zone.ni.com/reference/en-XX/help/370052AA-01/tshelp/infotopics/2019whatsnew/#quickdrop)
* Insert items from a dialog without using mouse clicks
* Features:
  * Steps (select adapter)
  * Callbacks
  * Variables (Locals, Parameters, File/StationGlobals)
    * Arrays
  * `<New Sequence>`
  * `Template *`

---
<!-- _header: TestStand **Quick Drop** -->

## TestStand **Quick Drop**

</br>

* Missing:
  * Add current sequence file sequences as steps
  * Shortcuts
  * **Tools** 😭

---
<!-- _header: TestStand **Quick Drop**-->

## Summary

</br>

A huge step forward, but more could be expected 😜

---

<!-- _class: invert -->

# **Custom** TestStand QuickDrop

![bg left w:95%](https://github.com/425J/GLA/blob/main/images/CTSQD.png?raw=true)

---
<!-- _header: '**Custom** TestStand QuickDrop' -->

</br>

> Don't Wait for NI R&D, Implement Your Own Features

<div style="font-size:50%;"></br>Inspired by</br><a href="https://education.ni.com/center-of-excellence/resources/1141/don-t-wait-for-labview-r-d-implement-your-own-labview-features">Don't Wait for LabVIEW R&D,</br>Implement Your Own LabVIEW Features</a></br>by Darren Nattinger</div>

---
<!-- _header: '**Custom** TestStand QuickDrop' -->

## How it work

</br></br>

* Polling loop checks [`ApplicationMgr.SequenceFiles`](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsuiref/reftopics/applicationmgr_sequencefiles_p/)
* If new sequence appears call [`ApplicationMgr.GetSequenceFileViewMgr`](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsuiref/reftopics/applicationmgr_getsequencefileviewmgr_m/) and get its `SequenceViewConnection` ➡ `SequenceView`
* Register [`KeyDown`](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsuiref/reftopics/sequenceview_keydown_e/) event for [`SequenceView`](https://zone.ni.com/reference/en-XX/help/370052AA-01/tsuiref/reftopics/sequenceview/) of each sequence file 🤯
* Display Tools Dialog if `SHIFT`+`SPACE` pressed
* List tools (use current sequence file context for `Expression.Evaluate`) and run selected one

---
<!-- _header: '**Custom** TestStand QuickDrop'-->

## Summary

</br>

![bg h:60%](https://en.meming.world/images/en/b/be/But_It%27s_Honest_Work.jpg)

---
<!-- _header: '**Custom** TestStand QuickDrop' -->

# [Custom TestStand QuickDrop Repo](https://github.com/425J/SeqEditQuickDrop)

---
<!-- _class: invert -->

# Thank You