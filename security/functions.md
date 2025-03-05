# Security Functions
Sanitisation
------------

All these sanitisation functions can be used to sanitise data before further processing, and to make data safe to be inserted into the database.

### sanitize\_text\_field()

The main usage of [`sanitize_text_field()`](https://developer.wordpress.org/reference/functions/sanitize_text_field/) function is to sanitise the data provided by text input fields in forms. But it’s useful for sanitising any kind of data that you want to be plain text.

`sanitize_text_field()` applies the following modifications to the data:

*   Removes all HTML tags.
*   Removes whitespace from the start and end of the string.
*   Removes extra whitespace (more than a single space) between words.
*   Removes tabs and line breaks.
*   Converts stand-alone `<` characters into an HTML entity.
*   Removes any invalid UTF-8 characters.
*   Removes `%` encoded octets.

### absint()

[`absint()`](https://developer.wordpress.org/reference/functions/absint/) is a useful function for sanitizing IDs: they need be an absolute integer, meaning a whole number that’s positive or zero.

`absint()` is a wrapper function for two PHP functions: [intval()](http://php.net/manual/en/function.intval.php) turns the data into an integer, and [abs()](http://php.net/manual/en/function.abs.php) makes sure that it is an absolute value. If you need to sanitise an integer that can be negative or positive, use the PHP function `intval()`. It will only cast the data to an integer.

Integers are safe to use in any context. When you pass invalid data (like a text string) to `absint()`, the return is most likely a 0. As the function internally converts the data into an integer, the rules of [integer casting](http://php.net/manual/en/language.types.integer.php#language.types.integer.casting) apply.

In MySQL IDs start at 1, so it’s a good practice to check whether any IDs are higher or equal to 1 before proceeding.

### esc\_url\_raw()

[`esc_url_raw()`](https://developer.wordpress.org/reference/functions/esc_url_raw/) sanitises URLs for safe storage in a database by stripping undesired characters and verifying the URL protocol.

The function accepts two arguments: the URL to clean, as well as an optional array of allowed protocols. URLs that don’t use the whitelisted protocol(s) will be discarded.

So if you only want to save URLs that start with `https://`, you can call the function like this:

```php
$clean_url = esc_url_raw( $url, [ 'https' ] );


```


Keep in mind that relative URLs starting with a /, #, or ?, as well as file names ending with .php will not be discarded by esc\_url\_raw(). So if you need an absolute URL to a website, you need to put additional checks into place.

### remove\_accents()

As the name implies, [`remove_accents()`](https://developer.wordpress.org/reference/functions/remove_accents/) replaces accents with their ASCII equivalents.

### sanitize\_email()

The [`sanitize_email()`](https://developer.wordpress.org/reference/functions/sanitize_email/) function performs a number of checks to detect invalid email address formats, and strips undesired characters.

It returns an empty string when the basic validity checks fail. If the email address has the right format, the sanitized address is returned.

### sanitize\_file\_name()

The [`sanitize_file_name()`](https://developer.wordpress.org/reference/functions/sanitize_file_name/) function applies the following modifications to the data:

*   Ensures that filenames are valid on all operating systems supported by WordPress.
*   Ensures that filenames are command line friendly.
*   Removes prepended and appended period, dash, and underscore characters.
*   Replaces multiple dashes in a row with a single dash.
*   Replaces spaces with a dash.
*   Adds an underscore to intermediate extensions that are not whitelisted.

`sanitize_file_name()` only handles sanitizing the name of the file. It doesn’t make sure that the name is unique, you would need to use [`wp_unique_filename()`](https://developer.wordpress.org/reference/functions/wp_unique_filename/) for that.

While it handles intermediate extensions, it is not concerned with the main extension of the file. As an example, `file.exe.exe` will be transformed into `file.exe_.exe`, because `.exe` is not an allowed extension. `file.exe` will not be modified though.

You should also use [`wp_check_filetype()`](https://developer.wordpress.org/reference/functions/wp_check_filetype/) to verify that the extension of the file is allowed on the system. The function returns an array with two keys: `ext` and `type`. Both will be set to `false` if the file type is not part of the allowed [MIME types](https://en.wikipedia.org/wiki/Internet_media_type).

### sanitize\_key() and sanitize\_title()

The [`sanitize_key()`](https://developer.wordpress.org/reference/functions/sanitize_key/) and [`sanitize_title()`](https://developer.wordpress.org/reference/functions/sanitize_title/) both convert data to slug form.

Slugs can only be composed of lowercase alphanumeric characters (characters from a to z and numbers from 0 to 9), dashes (`-`) and underscores (`_`). Slugs are safe to use in any HTML or URL context.

The difference between the two functions is that `sanitize_key()` is more aggressive and removes any non alphanumeric characters. It is therefore the right function to use for sanitizing slugs used to interact with the database.

`sanitize_title()` tries to preserve more of the initial string by converting unacceptable entities instead of simply removing them. The function accepts a `$context` argument that determines which replacements are done. The strictest context is `save`, which creates a slug that is appropriate to be saved to the database.

first replaces accent characters with their ASCII equivalents, before transforming the data into slug form.

### sanitize\_title\_with\_dashes()

The [`sanitize_title_with_dashes()`](https://developer.wordpress.org/reference/functions/sanitize_title_with_dashes/) is used by the `sanitize_title()` function. It transforms text into slug form, either for display or for saving to the database.

The main difference between `sanitize_title()` and `sanitize_title_with_dashes()` is that with the `save` context, accents will be removed instead of replaced with their ASCII equivalents.

### wp\_check\_invalid\_utf8()

[`wp_check_invalid_utf8()`](https://developer.wordpress.org/reference/functions/wp_check_invalid_utf8/) checks whether a string is valid UTF8. By default invalid strings will be discarded, and an empty string will be returned.

The function can also attempt to convert invalid strings using PHP’s `iconv()` function if the `$strip` argument is set to true.

Validation
----------

### WordPress validation functions

*   [`is_email()`](https://developer.wordpress.org/reference/functions/is_email/): Checks whether the data is a valid email address. The validation done by the function does not comply with the [RFC 822](http://www.faqs.org/rfcs/rfc822.html) standard, and does not work with internationalized domain names.
*   [`wp_validate_boolean()`](https://developer.wordpress.org/reference/functions/wp_validate_boolean/): Despite the name, this function not only validates, but also sanitizes the data passed to it. So the return value will always be a boolean. You can use `filter_var( $var, FILTER_VALIDATE_BOOLEAN, FILTER_NULL_ON_FAILURE )` as an alternative, as it returns `null` when the passed data is not valid.
*   [`sanitize_hex_color()`](https://developer.wordpress.org/reference/functions/sanitize_hex_color/): This actually a validation function, as it returns null if the color code isn’t valid. It is only available in the Customizer context, but it’s a small function so you can copy the code to your own validation function if needed.
*   [`sanitize_hex_color_no_hash()`](https://developer.wordpress.org/reference/functions/sanitize_hex_color_no_hash/): The same as `sanitize_hex_color()` but for values without a leading `#`.
*   `rest_is_ip_address()`
    
*   `rest_is_boolean()`
    
*   `is_serialized()`
    

### PHP validation functions

PHP offers a number of validation functions. As we have seen previously, using them can be a bit tricky. So make sure to read the documentation carefully, including the notes.

*   [`is_bool()`](http://php.net/is_bool): Returns true if the passed variable is of the type boolean.
*   [`is_float()`](http://php.net/manual/en/function.is-float.php): Returns true if the passed variable is of the type float.
    
*   [`is_int()`](http://php.net/is_int): Returns true if the passed variable is of the type integer.
    
*   [`is_numeric()`](http://php.net/manual/en/function.is-numeric.php): Returns true if the passed variable contains a numeric value. Keep in mind that this encompasses all numeric values, so signs, hexadecimal, binary, and octal values are all valid.
    
*   [`strtotime()`](http://php.net/manual/en/function.strtotime.php): Not a validation function strictly speaking, but can be used as such to validate dates. The function returns false if the passed data cannot be converted into a timestamp.
    

Escaping functions
------------------

### esc\_html()

The `esc_html()` function ensures that data is plain text, and safe to be output between HTML tags.

### esc\_attr()

`esc_attr()` ensures that data is safe to be output inside of HTML attributes.

### esc\_js()

`esc_js()` is probably the most misused escaping function: its only use is to make data safe to be output inside a JavaScript string. The function is designed with inline JavaScript placed into HTML attributes in mind. The escaped data therefore needs to be contained inside of single quotes.

Inline JavaScript is not a part of modern developing techniques, so this function should rarely if ever be used in new code.

### wp\_json\_encode()

`wp_json_encode()` is a wrapper for PHP’s `json_encode()`. It ensures that data is safe to be printed inside of JavaScript code.

Data that is escaped with `wp_json_encode()` does not need to be wrapper in quotes, as the function takes care of that.

Keep in mind that the function only makes data passed from PHP to JavaScript safe to be embedded into `<script>` tags. It has no security implications on the JavaScript side, meaning that the data still needs to be escaped on output (when added to the DOM) in JavaScript.

Correct Function Usage
----------------------

### in\_array()

Always set the $strict argument to `true`, to enable strict comparison. Loose type comparison can lead to unexpected results.
