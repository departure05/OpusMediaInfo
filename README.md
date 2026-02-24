# OpusMediaInfo
All the MediaInfo data you can ask for Opus!

`OpusMediaInfo` is a script add‑in for [Directory Opus](https://www.gpsoft.com.au) that extends the default multimedia columns, letting you pull in any MediaInfo value right into Opus. What really sets it apart is its intuitive UI. No more manual edits just to add some columns. Just preview and pick exactly the data you need, then choose how it'll appear in Opus.

#### **Key Features:**

* Full support for virtually every property exposed by MediaInfo, including unofficial ones.
* Easy to use. Just open the config dialog with a file, or drop a file in, pick the properties you want to add by their preview value, and you're good to go.
* A built-in UI to manage which columns you want to add : customize the name, header, type, category, applicable extensions, and more.
* Designed for both casual and advanced users: the interface is simple but powerful enough to define custom rules and formatting.
* Supports both single-value and multi-stream outputs at runtime, no need to tweak the script manually.
* Real-time preview of values in each property, as they'd appear in the file display.
* Fully customizable [infotips](#infotips) with special entries that show results live.
* Support for [custom columns](#custom-columns) that can take their value from multiple MediaInfo fields in a single column.
* Easily switch data types on the fly.
* Import/export your configuration easily.
* Data can be retrieved very fast.
* Support for regular Mediainfo CLI.
* Support localization for values.
* Built-in support for [caching values](#about-cache) based on a configurable file size, with automatic file change detection.
* Dump Mediainfo data for desired files through different formats.
* Much more...

<img width="1500" height="812" alt="column_editor" src="https://github.com/user-attachments/assets/0888a0b4-1645-4c9c-abca-4b08b4647923" />

---

#### **Installation:**

This script fully support `DOpus-Scripting-Extensions` by @PolarGoose. First, you'll need to install the latest version from [here](https://github.com/PolarGoose/DOpus-Scripting-Extensions/releases).
Additionally, you can also use the regular Mediainfo.exe CLI (including localization files), and put it in `/dopusdata\User Data\OpusMediaInfo`.

Then install the script as usual. (Required at least DOpus v13.21.1).


<a name="release">Latest version</a> : [Download from here](https://github.com/departure05/OpusMediaInfo/releases/download/latest/OpusMediaInfo.opusscriptinstall)

You can always find the latest version published [here](https://github.com/departure05/OpusMediaInfo).

---

#### **Columns Editor**

The first time you install the script, it'll ask if you want to start configuring columns, assuming you already have MediaInfo set up correctly. If not, it'll give you some options to continue.

<img width="646" height="267" alt="welcome" src="https://github.com/user-attachments/assets/2ea4f16d-650b-4808-b0cd-708b7b400967" />

To configure which columns are enabled, you need to open the configuration dialog, which you can do in a few different ways:

* From the Script Management window, click the Configure button.
* Using the `OpusMediaInfo CONFIG` command. You can also include the fullpath to a file you'd like to preview.

The configuration dialog has two tabs : one for `column` settings and another for `infotips`.

When this dialog opens, if there's no columns enabled, you'll be asked to select a file so MediaInfo can fill in the list with values from its inform.

<img width="1500" height="812" alt="no_columns" src="https://github.com/user-attachments/assets/9f03f9c1-a7d2-4893-afd7-aa2656d0d9c0" />

<img width="1500" height="812" alt="column_editor" src="https://github.com/user-attachments/assets/0888a0b4-1645-4c9c-abca-4b08b4647923" />

Checked properties are the ones that will be enabled as columns in Opus once you close the dialog.

MediaInfo groups each field into one of the following: General, Video, Audio, Text, Menu, Image, and Other. Each property belongs to only one group, and there can be properties with the same name across different groups. Only properties in the "General" group have a single value; others can have multiple values, one for each stream in the file.
Using the controls on the right, you can edit each column's label, header, type and so on.

The "Multiple" control lets you choose whether the column will show the property for each stream in the group, or just the first one. If "Multiple" is set to "True", the chosen separator will be used to split the values. The list has the "#" column, which shows how many values a given keyword has, useful for deciding its value.

The "Type" control sets the data type of the column's content. Allowed types are: String (text), number (int), double, star (rating), datetime, date, time, duration, and size.
You can change this value and immediately see how it affects the preview below.
Note: When "Multiple" is **True**, the type is always treated as String, since it's best practice not to mix value types in a single column.

The "Category" control lets you change where the column appears. By default, columns are added under Script > OpusMediaInfo. You can switch this to any other category you prefer. This doesn't affect the behavior or output of the column.

The "Pattern" controls let you apply advanced formatting using regular expressions.
By default, the script uses predefined formats to adjust the type of each value. You can override those with your own.
The first field is where you define the actual pattern to match. Use the same format as ECMAScript literal regex: `/pattern/flags`.
The second field defines the replacement string. Both follow the same format used in JScript's `String.replace(pattern, replace)`, so compatibility depends on that context.

The listed extensions below are used to determine which files the column applies to.
You can add file type groups using `grp:` followed by the internal or display name of the group.
You can also list specific extensions (include the dot). Wildcards are not supported.

Every time you open a file for preview, the script saves the listed properties for later use, so you don't have to remember which file had which properties when adding a new column.

You can Export or Import columns from this same window, using the provided buttons.

<img width="721" height="265" alt="import_export" src="https://github.com/user-attachments/assets/5427ec50-4c42-4473-9849-900a2a1be755" />

For keyboard navigation, the following hotkeys are active when the list is focused:
<kbd>Ctrl</kbd>+<kbd>L</kbd>: Focuses the Label field
<kbd>Ctrl</kbd>+<kbd>H</kbd>: Focuses the Header field
<kbd>Ctrl</kbd>+<kbd>T</kbd>: Focuses the Type field
<kbd>Ctrl</kbd>+<kbd>M</kbd>: Focuses the Multiple field
<kbd>Ctrl</kbd>+<kbd>G</kbd>: Focuses the Category field
<kbd>Ctrl</kbd>+<kbd>R</kbd>: Focuses the Pattern field
<kbd>Ctrl</kbd>+<kbd>A</kbd>: Focuses the Alignment field

You can also press <kbd>F3</kbd> to move focus to the Filter field.

After closing the dialog, the columns will be added automatically and will be ready to use in Opus, in any field where they apply.
Note: If the columns are already visible, you may need to refresh the lister to see the changes.


You can also access the Options dialog from here, which lets you configure things like the script log level, the separator used for multiple values, and settings related to [cache usage](#about-cache).

<img width="802" height="417" alt="options" src="https://github.com/user-attachments/assets/79ae1ea7-9e17-49be-858e-69093a2b085a" />

(This dialog is also accesible from the Script Management window, by clicking the About button).

---

### Infotips

<img width="1500" height="812" alt="infotips" src="https://github.com/user-attachments/assets/ed41538f-0c95-4702-aa64-db44429a94e8" />

You can create custom infotips for each group and then simply reference them in Opus using their specific keyword.
Available infotips are for: General, Audio, Video, Text, Menu, and Image.

When building infotips, to get the keyword you want, select it from the list and press <kbd>Ctrl</kbd>+<kbd>C</kbd> to copy it to the clipboard.
Infotips use a special syntax, which basically works like this:

```
{keyword|value if not available|specific format}
```

Where parts 2 and 3 are optional.
The specific format expects "%1" to insert the keyword's value in the desired position.
If the keyword doesn't exist, or the text isn't inside {}, the text is passed as is.
These values support all the markup code that Opus allows.

---

### **Custom Columns**

Since v2.0.0, you can create custom columns that pull values from multiple MediaInfo fields. You can pick multiple MediaInfo fields and order them so the script takes the first value found and shows it as the column value. That way you can have a single column that groups several sources and adapts depending on the file. For example, you can create a column that shows the width of the first video stream for video files, and the image width for image files. There are lots of possibilities.

These columns have the same customization options as the others (type, multiple or not, etc.). The only difference is that their value can depend on multiple fields instead of just one.

To create them, press the "Custom" button in the main UI. That opens a dialog that asks for the name (keyword) of the new column and lets you pick the fields to pull data from and define the processing order.

<img width="1500" height="812" alt="custom_column" src="https://github.com/user-attachments/assets/908d421e-a113-45c5-8ba1-cdee421366f3" />

You can also edit existing custom columns using the "Edit..." button. (Only appears when a Custom column is selected).

---

#### **Command's Arguments**

The command support the following arguments:

| Argument | Type | Value | Desc |
|----|----|----|----|
| **CONFIG** | /O |  | Shows the dialog windows for configuring columns/infotips. |
|  |  | *filepath* | Accepts a filepath to populate the preview column right after start. |
| **EXPORT** | /S |  | Exports columns/infotips to a file. |
| **IMPORT** | /O |  | Imports columns/infotips from a file without deleting existing ones. If no file is specified, you'll be prompted to choose one. |
| **!IMPORT** | /O |  | Imports columns/infotips from a file, deleting existing ones. If no file is specified, you'll be prompted to choose one. |
| **DUMP** | /M/O |  | Save the MediaInfo report for the chosen items to a file. You can pass multiple filenames separated by spaces (use commas if a filename contains a space). If no files are passed, the command will use the selected items in the source folder.|
| **AS** | /K |  | Used together with `DUMP`, this lets you specify the output format. Valid values are: json,xml,txt,csv,html.|
| **TO** | /K |  | Used together with `DUMP`, this lets you specify the outpath path.|
| **PURGEDB** | /O |  | Performs a database cleanup, if applicable.|
|  |  | *path/wildcard* | If a text value is specified, it can be the full name of a specific file or a path with * and ? wildcard support. If no value is specified, it removes all values for non-existing items (`KEEPMISSINGDRIVES` can change this behavior). |
| **KEEPMISSINGDRIVES** | /S |  | Used together with `PURGEDB` (without a specified value), it allows you to keep missing entries that are located on unavailable drives (e.g. on an external disk).|
| **VACUUM** | /S |  | Compacts the database. Use together with `PURGEDB`.|

---

#### **Importing from a file**

The command supports importing columns directly from a valid JSON file encoded with UTF-8 NOBOM, with the following format:

<details>

There are two sections: **"columns"** and **"infotips"**.

Each entry (KEY) in both groups must have the following properties:

* **"label"**: Cannot be empty.
* **"header"**:
* **"align"**: Must be `left`, `center`, or `right`.
* **"type"**: Must be `string`, `number`, `number,signed`, `size`, `double`, `duration`, `durationh`, `graph`, `igraph`, `percent`, `datetime`, `date`, or `time`.
* **"category"**: Must be `music`, `movie`, `image`, `script`, `size`, `std`, `date`, or `sums`.
* **"enabled"**: Must be `1` or `0`.
* **"exts"**: Cannot be empty; must include at least one `.` or `grp:`. Multiple values are separated by `; `.
* **"group"**: Must be `General`, `Audio`, `Video`, `Image`, `Text`, `Menu`, `Other` or `Custom`.
* **"keyword"**: Cannot be empty (except for infotips).
* **"multi"**: Must be `true` or `false`.
* **"regex"**: The regex used for custom formatting.
* **"regex1"**: The regex flags.
* **"regex2"**: The replacement value for the regex.
* **"value"**: Only for infotips and custom columns. Cannot be empty.

Additionally:

* **KEY** must follow the internal format for columns (`group_keyword`) or infotips (`group_infotip`).

E.g.
```json
{
	"columns": {
		"Audio_Default": {
			"label": "Audio Default",
			"header": "Default",
			"align": "left",
			"type": "string",
			"category": "script",
			"enabled": 1,
			"exts": "grp:Video",
			"group": "Audio",
			"keyword": "Default",
			"multi": true,
			"regex": "",
			"regex1": "",
			"regex2": ""
		}
	},
	"infotips": {
		"General_infotip": {
			"label": "General_infotip",
			"header": "General_infotip",
			"align": "left",
			"type": "string",
			"category": "script",
			"enabled": 1,
			"exts": "grp:Music; grp:Movies; grp:Images",
			"group": "General",
			"keyword": "",
			"multi": true,
			"regex": "",
			"regex1": "",
			"regex2": "",
			"value": "{FileSize_String||Size: %1},{OverallBitRate_String|| Bitrate: %1}{Duration_String||, Duration: %1} {File_Created_Date||Created: %1} {Encoded_Date||Encoded: %1} {Released_Date||Released: %1}"
		}
	}
}
```
</details>

---

#### **About cache**

Since v4.0, the script includes optional built-in support that allows you to store data extracted with MediaInfo in a database, so when you need it again, it can be retrieved easily and much faster.
This option is enabled by default and can be changed from the Options window, accessible from the Column Editor or by pressing the About button in the Opus Script Management.
To use this feature, you need to have the [Microsoft Access Database Engine](https://www.microsoft.com/en-us/download/details.aspx?id=54920) installed, which usually comes with Microsoft Office.
If the script detects that the engine is not installed, you will not be able to enable or use this optional feature.
Additionally, you can decide the minimum file size for files whose data will be read and stored in the database.
You can also decide whether this option should be available when Opus is running from a USB export.
There is also be a way to define exclusions with wildcard support, so you can ignore certain files and prevent them from being added to the database.

<img width="802" height="417" alt="mi_database_options" src="https://github.com/user-attachments/assets/0550707a-6744-4dbc-87a9-f178c4173533" />

---

#### **Notes:**

* Starting with version 16.0 of `DOpus-Scripting-Extensions`, unit values and certain words will be displayed in the same language Opus is running in.
* The script adds certain values to the "Menu" group for convenience: `ChaptersCount` and `ChaptersList`.
* The script adds the field `Dimensions` for every stream in the "Video" and "Image" groups. (Its value is `Height` x `Width` x `Bitdepth`)
* Starting with v4.2, the script adds some common predefined columns with suggested file types. Feel free to edit them, and you can still add more entries to the list whenever you drop a file into it.
* Since v3.0.0, conformance tags are supported. You can find the total count for all conformance issues in `General_ConformanceErrors` and `General_ConformanceWarnings`. This data can be very useful for finding damaged multimedia files or files that have some kind of issue.
* You can access to the Options dialog by clicking the About button from the Script Management window.
* For the database cache feature, the script can automatically detect most file changes and update their values in the database by using a file sampling technique that performs micro MD5 checksums. Note that for some plain text files, this technique may fail to detect changes.
* Columns with the same label will be modified internally to avoid duplicates.
* Custom columns can't be used for other custom columns or infotips.
* When using `OpusMediaInfo PURGEDB`, note that some paths (e.g., slow remote ones) may take longer to be checked for existence. During this time, the database will not be available.

---

#### **Acknowledgments/Credits:**

* [MediaInfo](https://mediaarea.net/SupportUs).
* [@PolarGoose](https://github.com/PolarGoose), for the awesome plugin that makes this project possible.
* Main icon adapted from [The Mightiest Icons on the Web](https://glyphs.fyi)
* Opus devs, for an amazing program that goes beyond a file manager.
