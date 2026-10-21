# OpusMediaInfo
All the MediaInfo data you could ever want in Opus!

`OpusMediaInfo` is a script add-in for [Directory Opus](https://www.gpsoft.com.au) that extends the default multimedia columns, letting you access any MediaInfo value directly in Opus. What really sets it apart is its intuitive UI. No more manual edits just to add columns. Simply preview and select the data you need, then choose how it should appear in Opus.

> NOTE: Since v6.0, `OpusMediaInfo` has used its own Go-based helper and `MediaInfo.dll`.
> Versions prior to v6.0 require [DOpusScriptingExtensions](https://github.com/PolarGoose/DOpus-Scripting-Extensions). Older versions are considered deprecated.

#### **Key Features:**

* Full support for virtually every property exposed by MediaInfo, including unofficial ones.
* Easy to use. Just open the config dialog with a file, or drop a file into it, select the properties you want to add based on their preview values, and you're good to go.
* A built-in UI for managing columns: customize their names, headers, types, categories, applicable extensions, and more.
* Designed for both casual and advanced users: the interface is simple yet powerful enough to define custom rules and formatting.
* Supports both single-value and multi-stream outputs at runtime, with no need to modify the script manually.
* Real-time preview of each property's values as they would appear in the file display.
* Fully customizable [infotips](#infotips), with special entries that display live results.
* Support for [custom columns](#custom-columns) that can retrieve values from multiple MediaInfo fields in a single column.
* Easily change data types on the fly.
* Easy configuration import/export.
* Very fast data retrieval.
* Support for localized values.
* Built-in support for [caching values](#database) through a SQlite database, based on a configurable file size, with automatic file change detection.
* Dump MediaInfo data for selected files in various formats.
* Much more...

<img width="1561" height="967" alt="editor" src="https://github.com/user-attachments/assets/4a4435c3-3346-4f0a-9cf6-af6573dabce2" />


---

#### **Requirements:**

* Windows 10 1803 or higher (required for communication with the helper).
* Directory Opus 13.25.4 or higher.
* `MediaInfo.dll`.
  - It's already included in the installer. You can also obtain it [here](https://mediaarea.net/en/MediaInfo/Download/Windows) (choose the 64-bit DLL version without the installer).
  - Alternatively, if you already have it somewhere on your system, you can specify its path in Script Options.
* If you need localization (for non-English users), place the corresponding `.csv` language files alongside the DLL or the helper. It's recommended to do this before the database starts being populated.
  - Language files can be installed through the installer.

---

#### **Installation:**

This script comes with its own installer, which you can download [here](https://github.com/departure05/OpusMediaInfo/releases/download/latest/OpusMediaInfoInstaller.exe).

The installer will put the helper in place and automatically launch the script installation. Optionally, it can also copy `MediaInfo.dll` and the language files.

If, for some reason, the installer cannot start the script installation, you can always find the latest `.opusscriptinstall` package and the helper [here](https://github.com/departure05/OpusMediaInfo/releases/download/latest).

The first time you install the script, you'll be asked if you want to start configuring columns, assuming `MediaInfo.dll` is already set up correctly. If it isn't, the script will show you where to download it so you can continue.

<img width="775" height="467" alt="welcome" src="https://github.com/user-attachments/assets/3ae30522-ffef-46c1-946b-ec4a526a82f5" />


Once you've placed `MediaInfo.dll` in the appropriate location, you're good to go.

Alternatively, you can open the `Script Management` window and set the path to your existing DLL under the `MediaInfo && DB` tab.

---

#### **Columns Editor**

To configure which columns are enabled, open the configuration dialog in one of the following ways:

* From the Script Management window, click the Configure button.
* Run the `OpusMediaInfo CONFIG` command. You can also include the full path to a file you want to preview.

The configuration dialog has two tabs: one for column settings and another for infotips.

When the dialog opens, if no columns are enabled, you'll be asked to select a file so MediaInfo can populate the list with values from its report.

<img width="1930" height="827" alt="no_cols" src="https://github.com/user-attachments/assets/a7654668-3e44-4965-94da-396e56a12bac" />

Checked properties will be enabled as columns in Opus once you close the dialog.

MediaInfo groups each field into one of the following categories: General, Video, Audio, Text, Menu, Image, and Other. Each property belongs to only one group, and properties with the same name can exist in different groups. Only properties in the "General" group have a single value; properties in other groups can have multiple values, one for each stream in the file.

Use the controls on the right to edit each column's label, header, type, and other settings.

The **Multiple** control lets you choose whether the column displays the property for every stream in its group or only the first one. If **Multiple** is set to `True`, the selected separator will be used to join the values. The list includes a `#` column showing how many values a given keyword has, which is useful when deciding which properties to use.

The **Type** control sets the data type of the column's content. Allowed types are: String (text), number (int), double, star (rating), datetime, date, time, duration, and size.

You can change the type and immediately see how it affects the preview below.

Note: When **Multiple** is set to `True`, the type is always treated as String, since mixing value types in a single column is not recommended.

The **Category** control lets you choose where the column appears. By default, columns are added under Script > OpusMediaInfo. You can change this to any category you prefer. This doesn't affect the column's behavior or output.

The **Pattern** controls let you apply advanced formatting using regular expressions.

By default, the script uses predefined formats to adjust the type of each value, but you can override them with your own.

The first field defines the pattern to match. Use the same format as an ECMAScript regex literal: `/pattern/flags`.

The second field defines the replacement string. Both follow the format used by JScript's `String.replace(pattern, replace)`, so compatibility depends on that context.

The extensions listed below determine which files the column applies to.

You can add file type groups by entering `grp:` followed by the group's internal or display name.

You can also list specific extensions (including the dot). Wildcards are not supported.

Whenever you open a file for preview, the script saves the listed properties for later use, so you don't have to remember which file had which properties when adding a new column.

You can export or import columns from this window using the provided buttons.

You can also choose which columns should have their contents blurred when using Directory Opus's Secure Screenshot feature.

For keyboard navigation, the following hotkeys are available when the list has focus:

- <kbd>Ctrl</kbd>+<kbd>L</kbd>: Focuses the Label field.
- <kbd>Ctrl</kbd>+<kbd>H</kbd>: Focuses the Header field.
- <kbd>Ctrl</kbd>+<kbd>T</kbd>: Focuses the Type field.
- <kbd>Ctrl</kbd>+<kbd>M</kbd>: Focuses the Multiple field.
- <kbd>Ctrl</kbd>+<kbd>G</kbd>: Focuses the Category field.
- <kbd>Ctrl</kbd>+<kbd>R</kbd>: Focuses the Pattern field.
- <kbd>Ctrl</kbd>+<kbd>A</kbd>: Focuses the Alignment field.

You can also press <kbd>F3</kbd> to move focus to the Filter field.

Press <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>T</kbd> to toggle the window's always-on-top status (only when opened via the command).

After you close the dialog, the columns will be added automatically and will be ready to use in Opus wherever they apply.

Note: If the columns are already visible, you may need to refresh the lister to see the changes.

---

#### **Options Window**

You can access the Options dialog from here to configure settings such as the script log level, the separator used for multiple values, and options related to [cache usage](#about-cache).

<img width="1057" height="642" alt="options1" src="https://github.com/user-attachments/assets/90ccc10c-4769-4376-bb34-c785389b076e" />

<img width="1057" height="642" alt="options2" src="https://github.com/user-attachments/assets/4ddcd8ac-61de-406f-b9c7-a92afba93620" />


(This dialog is also accessible from the Script Management window by clicking the About button.)

---

### **Infotips**

<img width="1561" height="967" alt="infotips" src="https://github.com/user-attachments/assets/88597f82-4eeb-4c2d-aff3-94e8676dbbf9" />

You can create custom infotips for each group and then reference them in Opus using their respective keywords.

Available infotip groups are: General, Audio, Video, Text, Menu, and Image.

To get the keyword you need when building infotips, select it from the list and press <kbd>Ctrl</kbd>+<kbd>C</kbd> to copy it to the clipboard.

Infotips use a special syntax that works as follows:

```
{keyword|value if not available|simple format|conversion/format as}
```

Parts 2, 3, and 4 are optional.

The simple format uses `%1` as a placeholder for inserting the keyword's value at the desired position.

If the keyword doesn't exist, or the text isn't enclosed in `{}`, the text is passed through unchanged.

These values support all markup codes allowed by Opus.

For `conversion/format as`, you can use the same logic as the Evaluator's [As function](https://docs.dopus.com/doku.php?id=reference:evaluator:as) to convert values to specific types.

For example, `{File_Created_Date||Created: %1|date}` uses the fourth part to convert the value to the date type, using the local date format.

---

### **Custom Columns**

You can create custom columns that retrieve values from multiple fields. Select multiple MediaInfo fields and arrange them in the desired order. The script will use the first value it finds as the column's value.

This lets you create a single column that combines multiple sources and adapts to the file type. For example, you could create a column that displays the width of the first video stream for video files and the image width for image files. The possibilities are extensive.

Custom columns have the same customization options as regular columns (type, Multiple, etc.). The only difference is that their values can come from multiple fields rather than a single one.

To create a custom column, click the **Custom** button in the main UI. This opens a dialog where you can enter the new column's name (keyword), select the fields to retrieve values from, and define their processing order.

<img width="829" height="824" alt="custom_column" src="https://github.com/user-attachments/assets/0120f374-a813-43fa-842d-681c0c69b966" />

You can also edit existing custom columns using the **Edit...** button, which appears only when a custom column is selected.

---

### **Database**

This script optionally uses an SQLite database for caching. It includes built-in support for storing data extracted by MediaInfo in a database so that subsequent requests can retrieve it quickly and efficiently.

Caching is enabled by default and can be configured in the Options window, accessible from the Column Editor or by clicking the About button in Opus Script Management.

You can also specify the minimum file size for files whose data should be read and stored in the database.

Additionally, you can choose whether caching should be available when Opus is running from a USB export.

You can also define exclusions using wildcards to ignore certain files and prevent them from being added to the database.

The script automatically determines whether a file's data needs to be updated using several metrics, including file size, modification date, and quick hash. This process is transparent to the user.

The script also provides commands for database maintenance and cleanup.

---

### **Cleaning the Database**

You can clean up the database using several command arguments provided by the script.

If you use `PURGEDB` with a wildcard (`*`, `?`) or without a value, the script searches for orphaned database entries, depending on the other arguments used. A list of matching files will be displayed, allowing you to choose which entries to delete and which to keep.

<img width="858" height="762" alt="purge_select" src="https://github.com/user-attachments/assets/2811af84-6ee4-49b5-b8b8-03df0db394b1" />

If you use it along with `FILES`, you can remove specific entries from the database in a more direct way.

<img width="785" height="659" alt="purge_files" src="https://github.com/user-attachments/assets/e3ef01ab-7154-4071-b6fd-1c8cf0ca4496" />


You can also include `VACUUM` to compact the database.


---

#### **Command Arguments**

The command supports the following arguments:

| Argument | Type | Value | Description |
| --- | --- | --- | --- |
| **CONFIG** | /O | | Displays the configuration dialog for columns and infotips. |
| | | *filepath* | Accepts a file path to populate the column preview immediately after startup. |
| **EXPORT** | /S | | Exports columns and infotips to a file. |
| **IMPORT** | /O | | Imports columns and infotips from a file without deleting existing entries. If no file is specified, you'll be prompted to choose one. |
| **!IMPORT** | /O | | Imports columns and infotips from a file, deleting existing entries. If no file is specified, you'll be prompted to choose one. |
| **DUMP** | /M/O | | Saves the MediaInfo report for the specified items to a file. You can pass multiple filenames separated by spaces (use commas if a filename contains spaces). If no files are specified, the command uses the selected items in the source folder. |
| **AS** | /K | | Used with `DUMP` to specify the output format. Valid values are `json`, `xml`, `txt`, `csv`, and `html`. |
| **TO** | /K | | Used with `DUMP` to specify the output path. |
| **PURGEDB** | /O | | Performs database cleanup, if applicable. |
| | | *path/wildcard* | If a text value is specified, it can be the full name of a specific file or a path containing `*` and `?` wildcards. If omitted, all entries for non-existing items are removed (`KEEPMISSINGDRIVES` can change this behavior). |
| **KEEPMISSINGDRIVES** | /S | | Used with `PURGEDB` without a specified value to keep entries located on unavailable drives, such as external disks. |
| **VACUUM** | /S | | Compacts the database. Use together with `PURGEDB`. |
| **FILES** | /M | | Used with `PURGEDB` to specifically delete files from the database. |
| **NOONTOP** | /S | | Used with `CONFIG` to prevent the window from opening on top (the default). |

---

#### **Importing from a File**

The command supports importing columns directly from a valid JSON file encoded in UTF-8 without a BOM, using the following format:

<details>

There are two sections: **"columns"** and **"infotips"**.

Each entry (key) in both sections must have the following properties:

* **"label"**: Cannot be empty.
* **"header"**:
* **"align"**: Must be `left`, `center`, or `right`.
* **"type"**: Must be `string`, `number`, `number,signed`, `size`, `double`, `duration`, `durationh`, `graph`, `igraph`, `percent`, `datetime`, `date`, or `time`.
* **"category"**: Must be `music`, `movie`, `image`, `script`, `size`, `std`, `date`, or `sums`.
* **"enabled"**: Must be `1` or `0`.
* **"exts"**: Cannot be empty; must include at least one `.` or `grp:`. Multiple values are separated by `; `.
* **"group"**: Must be `General`, `Audio`, `Video`, `Image`, `Text`, `Menu`, `Other`, or `Custom`.
* **"keyword"**: Cannot be empty (except for infotips).
* **"multi"**: Must be `true` or `false`.
* **"blurrable"**: Must be `true` or `false`.
* **"regex"**: The regex used for custom formatting.
* **"regex1"**: The regex flags.
* **"regex2"**: The replacement value for the regex.
* **"value"**: Only for infotips and custom columns. Cannot be empty.

Additionally:

* The key must follow the internal format for columns (`group_keyword`) or infotips (`group_infotip`).

For example:

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
            "blurrable": false,
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

#### **Notes**

* The script adds certain values for convenience: `Dimensions`, `ChaptersCount`, and `ChaptersList`.
* The script includes some common predefined columns with suggested file types. Feel free to edit them, and you can add more entries to the list whenever you drop a file into it.
* Conformance tags are supported. You can find the total count of all conformance issues in `General_ConformanceErrors` and `General_ConformanceWarnings`. This information can be useful for identifying damaged multimedia files or files with other issues.
* For database caching, the script can automatically detect most file changes and update the stored values using a file-sampling technique that performs micro MD5 checksums. Note that this technique may fail to detect changes in some plain-text files.
* Columns with duplicate labels will be renamed internally to avoid conflicts.
* Custom columns cannot be used in other custom columns or infotips.
* When using `OpusMediaInfo PURGEDB`, note that checking whether certain paths exist (e.g., slow remote paths) may take longer. During this time, no other operations will be available.
* When the database is enabled, the script includes and populates two additional columns: `Stored` (a Boolean property indicating whether the file has data in the database) and `Last Saved` (the date the file was added to or updated in the database).
* The database stores paths using the volume GUID instead of the drive letter. This is particularly useful for files on removable or USB drives, which are prone to changing drive letters.

---

#### **Acknowledgments/Credits**

* This product uses the MediaInfo library, Copyright (c) 2002–2026 MediaArea.net SARL. [Website](https://mediaarea.net).
* Main icon adapted from [The Mightiest Icons on the Web](https://glyphs.fyi).
* The Opus developers, for creating an amazing program that goes far beyond a file manager.

The Go helper uses the following third-party libraries:

* `github.com/goccy/go-json`
* `github.com/mattn/go-sqlite3`
* `golang.org/x/sys`

You can find the latest version and updates [here](https://github.com/departure05/OpusMediaInfo).
