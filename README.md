# Log Highlight for Sublime Text

[![Package Control](https://packagecontrol.herokuapp.com/downloads/Log%20Highlight.svg?style=flat-square)](https://packagecontrol.io/packages/Log%20Highlight)

Log Highlight helps to view a log (any type) with customizable error/warning syntax & color scheme.
Open `.log` file and run `Log Highlight` in the context menu or `Log Highlight : Highlight Error/Warning` in the command palette.

## Features
***********

 * Customizable Syntax & Color Scheme
 * Add Bookmarks Automatically for Navigating Errors/Warnings
 * Support Go To Position in a Log by `result_file_regex`
 * Search a Base Directory Automatically for Relative Path Link
 * Summary Output Panel
 * Auto Refresh for Multiple Logs
 * Continuous Tracking for Multiple Logs
 * Enable Build Window (Output Panel)

#### Customizable Syntax & Color Scheme

After changing syntax and color scheme in **settings**, run `Log Hightlight: Generate Custom Syntax & Theme` command. You may have to restart sublime text once.

#### Bookmarks

When errors/warnings found, it will add bookmarks for them. You can navigate easily with new key binding (`alt+pagedown/up`)

#### Go To Position

By double-click, you can go to positions of links like `"../../abc.cpp", 32` or `./abc.v line 234` in a log. For relative path, it may automatically search a base directory near the log file

#### Summary Output Panel

By default, it summarizes error/warning list in a new output panel at the bottom of window. It is useful to debug without monitoring the log file directly. But do not close the log file, it is needed to get the event. (default keymap - toggle : alt+f12, hide : ESC)

#### More Flexible Style Syntax

From v0.4.0, you can highlight links and quotes inside "begin regex", "end regex" and "match regex" by using special words `{{{LINK}}}`, `{{{QUOTE}}}`. It can be used for the following gcc style error/warning message : `./src/abc.cpp:40:2 error: unknown escape seque ...`

#### Auto Refresh for Multiple Logs

From v0.5.0, when the log files are updated, it automatically refreshes the bookmarks, summary output panel. There's some inertial delays for smooth action.

#### Continuous Tracking for Multiple Logs

From v0.5.2, if there are open files which already log-highlighted when sublime text restart, it will track all again. (ST3 only)

#### Enable Build Window (Output Panel)

From v0.8.0, Log Highlight can be used for Build Window or Unsaved View (need to set `"log_ext": [ ".*"]`). But relative path link won't be used because the absolute path is unknown. In order to use relative path, you should set like the following: `output_view.settings().set('filepath', [PATH])` output_view is the handle of your output panel view.

#### Restore Settings :

Just remove `Packages/User/Log Highlight.tmLanguage`, `Log Highlight.hidden-tmTheme`

#### Settings :

```java
{
	// enable context menu
	"context_menu": true,

	// log file extension
	"log_ext": [ ".log" ], // use ".*" for all file extensions

	// error pattern set (regular expression)
	// [ "begin regex", "end regex" ] or [ "match regex", "" ]
	//
	// non-regex special word (sensitive case) : {{{LINK}}}, {{{QUOTE}}}
	//  -> They will be replaced with regex capturing groups in "begin regex", "end regex" or "match regex" for LINK regex, QUOTE regex
	//
	//  e.g.) ../src/foo.cpp:40 error: ../src/foo.cpp:40
	//      - You don't need to use {{{LINK}}} to highlight second `../src/foo.cpp` link, because it is inside begin ~ end
	//      - [ "^[\\r\\n].*?(?i)error", "[\\r\\n]" ] is enough for 2nd link
	//      - For 1st link, the former setting can't, because it is outside begin ~ end
	//      - You have to put {{{LINK}}} like [ "^{{{LINK}}}?[^\\r\\n]*?(?i)error", "[\\r\\n]" ]

	"error_enable": true,
	"error_pattern": [
		[ "^Error-\\[", "^\\s*[\\n]" ],                      // Error-[ ~ next empty line (multi-line)
		[ "^{{{LINK}}}?[^\\r\\n]*?(?i)error", "[\\r\\n]" ],  // a line including case-insensitive 'error' with or without a link in front of 'error'
		[ "^\\w+:\\s*\\*E", "[\\r\\n]" ]                     // ...: *E ... (single line)
		// <-- Remove, Change, Add More Patterns Here -->
	],

	// warning pattern set (regular expression)
	//  -> see error pattern description

	"warning_enable": true,
	"warning_pattern": [
		[ "^Warning-\\[", "^\\s*[\\n]" ],                    // Warning-[ ~ next empty line (multi-line)
		[ "^{{{LINK}}}?[^\\r\\n]*?(?i)warning", "[\\r\\n]" ],// a line including case-insensitive 'warning' with or without a link in front of 'warning'
		[ "^\\w+:\\s*\\*W", "[\\r\\n]" ]                     // ...: *W ... (single line)
		// <-- Remove, Change, Add More Patterns Here -->
	],

	// info pattern set (regular expression)
	//  -> see error pattern description

	"info_enable": false,
	"info_pattern": [
		[ "^Information-\\[", "^\\s*[\\n]" ],                // Information-[ ~ next empty line (multi-line)
		[ "^\\[INFO\\]", "[\\r\\n]" ]                        // [INFO] ... (single line)
		// <-- Remove, Change, Add More Patterns Here -->
	],

	// theme color set
	"theme_color": [
		"#0D1011", // background
		"#D7D7D7", // foreground
		"#FF8801", // caret
		"#555555", // selection
		"#555555", // selectionBorder
		"#272822", // lineHighlight
		"#F92672", // error message
		"#E6DB74", // link in error message
		"#66D9EF", // quote in error message
		"#A6E22E", // warning message
		"#FD971F", // link in warning message
		"#66D9EF", // quote in warning message
		"#70991f", // info message
		"#b36915", // link in info message
		"#428a99", // quote in info message
		"#D7D7D7"  // summary title
	],

	// summary panel
	"summary_panel": true,                 // show summary panel
	"summary_error_only": false,           // display only errors in the summary panel
	"summary_show_log_name": true,         // display log name
	"summary_update_for_last_log": false,  // update summary panel only for lastly log-highlighted log

	// bookmark
	"bookmark_enable": true,       // enable/disable bookmarks
	"bookmark_error_only": false,  // add bookmarks only for errors
	"bookmark_goto_error": true,   // automatically go to 1st error line

	// etc
	"search_base": true  // search base directory automatically
}
```

![Image of Log Highlight](https://raw.githubusercontent.com/poucotm/Links/master/image/log_highlight.gif)

## issues

When you have an issue, tell me through [https://github.com/poucotm/Log-Highlight/issues](https://github.com/poucotm/Log-Highlight/issues), or send me an e-mail poucotm@gmail.com, yongchan.jeon@samsung.com