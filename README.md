<div align="center">

## Number Converter


</div>

### Description

This utility shows how to convert numbers to different types (i.e. hex, decimal, binary, octal).
 
### More Info
 
The code converts the current value to a decimal, and then from decimal to the selected type. I do this because PHP has built in functions to convert hexadecimal, octal, or binary to decimal, and decimal to hexadecimal, octal, or binary, but doesn't including functions to convert octal to hexadecimal, binary to octal, or anything fancy like that. The code also makes use of the eval statement which helped reduce the length of the code considerably.


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Josh Sherman](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/josh-sherman.md)
**Level**          |Intermediate
**User Rating**    |3.7 (11 globes from 3 users)
**Compatibility**  |PHP 4\.0
**Category**       |[Math](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/math__8-12.md)
**World**          |[PHP](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/php.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/josh-sherman-number-converter__8-582/archive/master.zip)





### Source Code

```
<?
/*
 * convert.php - Number conversion in PHP-GTK.
 *
 * Author: Josh Sherman
 * Purpose: Converts a number to a different type.
 * Usage: php -q conversion.php
 *
 */
// Check to see if the PHP-GTK extension is available.
dl( 'php_gtk.' . (strstr( PHP_OS, 'WIN') ? 'dll' : 'so'));
// Called when delete-event takes place, tells it to proceed.
function delete_event()
{
	return false;
}
// Called when the window is being destroyed, tells it to quit the main loop.
function destroy()
{
	Gtk::main_quit();
}
// Called when a radio button is clicked, converts the number to that format.
function convert($widget, $which)
{
	global $current_type;
	global $entry;
	// Get the value from the entry field
	$number = $entry->get_text();
	// Make sure they aren't clicking on an already active radio.
	if ($current_type != $which) {
		// Converts the number to decimal if it isn't already.
		if ($current_type != "dec") {
			eval ("\$number = " . $current_type . "dec(\"$number\");");
		}
		// Converts the number to the desired format.
		if ($which != "dec") {
			eval ("\$number = strtoupper(dec" . $which . "(\"$number\"));");
		}
		// Sets the entry box to the new value.
		$entry->set_text($number);
	}
	// Set the new type as the current type.
	$current_type = $which;
}
// Creates a new top-level window and connect the signals to the appropriate functions.
$window = &new GtkWindow();
$window->connect('destroy', 'destroy');
$window->connect('delete-event', 'delete_event');
$window->set_title("Conversion Utility");
$window->set_border_width(5);
$window->set_policy(false, false, false);
// Creates a table to place our widgets, and adds it to the table.
$table = &new GtkTable(2, 1);
$window->add($table);
// Creates an entry field, and places it on our table.
$entry = &new GtkEntry();
$table->attach($entry, 0, 1, 0, 1);
// Creates another table, and places it on the existing table.
$types = &new GtkTable(1, 4);
$table->attach($types, 0, 1, 1, 2);
// Creates and groups radio buttons.
$hex = &new GtkRadioButton(null, 'Hex');
$dec = &new GtkRadioButton($hex, 'Dec');
$oct = &new GtkRadioButton($hex, 'Oct');
$bin = &new GtkRadioButton($hex, 'Bin');
// Set the 'Decimal' radio as active, and set the current type to decimal.
$dec->set_active(TRUE);
$current_type = "dec";
// Connect the radios to the convert function, and feeds the value to it.
$hex->connect('pressed', 'convert', 'hex');
$dec->connect('pressed', 'convert', 'dec');
$oct->connect('pressed', 'convert', 'oct');
$bin->connect('pressed', 'convert', 'bin');
// Place the radios on the table.
$types->attach($hex, 0, 1, 0, 1);
$types->attach($dec, 1, 2, 0, 1);
$types->attach($oct, 2, 3, 0, 1);
$types->attach($bin, 3, 4, 0, 1);
// Create tool tips for the widgets and enabled them.
$tthex = &new GtkTooltips();
$tthex->set_delay(200);
$tthex->set_tip($hex, 'Convert the number to Hexadecimal.', '');
$tthex->enable();
$ttdec = &new GtkTooltips();
$ttdec->set_delay(200);
$ttdec->set_tip($dec, 'Convert the number to Decimal.', '');
$ttdec->enable();
$ttoct = &new GtkTooltips();
$ttoct->set_delay(200);
$ttoct->set_tip($oct, 'Convert the number to Octal.', '');
$ttoct->enable();
$ttbin = &new GtkTooltips();
$ttbin->set_delay(200);
$ttbin->set_tip($bin, 'Convert the number to Binary.', '');
$ttbin->enable();
$ttentry = &new GtkTooltips();
$ttentry->set_delay(200);
$ttentry->set_tip($entry, 'Type the number you want to convert here.', '');
$ttentry->enable();
// Show the window and all of it's child widgets.
$window->show_all();
// Run the main loop.
Gtk::main();
?>
```

