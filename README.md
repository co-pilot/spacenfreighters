Space 'n Freighters
===================

#Overview
I'm Open Sourcing some of the code behind the game Space 'n Freighters.  The first big chunk is the TextString class, something that's used internally to handle language translation, with flexible replacement parameters and proper pluralization.

This was written for a specific purpose, but could be useful in other AS3 projects - there's enough  flexibility in the class, so it should handle complex translations for large projects without any trouble.  However the call to convert text to different languages was designed to suit the needs of a simple game (with limited text) and is a little awkward.  After looking through the different options, the decision was made to break away from standards, which has some advantages, but also puts the burden of updating strings on the developer.

An internal ID system is used for translatable strings, this allows the English text to change without reverting translations (an issue with GetText, for example).  Strings that don't have plurals are easily handled and the mechanism also handles languages that need multiple plural forms (like Russian and Polish).  This is all handled seamlessly through a single 'translate' call.

The translatable text is stored using the XLIFF format, which allows translators to use existing tools like https://bitbucket.org/fredrik_corneliusson/transolution or you can use a services like https://www.transifex.com/.  The downside is the hassle of using ActionScript to read XML, since the calls will run asynchronously.  The class deals with this, callback functions can be passed around as needed (in an effort to decouple the code from Starling and reduce the code footprint with custom events).

Another side effect of the use in Starling is the languages that are supported, these were taken from the Starling Scaffold_mobil-app.xml file.  This means there's support for fourteen languages and Chinese only supports a single dialect.


#Basic Usage
An application always needs to set the language, there is no default and nothing will display properly otherwise:
```ActionScript
 = Capabilities.language;
```

After setting the language, text can be translated:
```ActionScript
TextString.translate(numItems, "string_id_value");
```

If the text doesn't have plural forms, then the first parameter is ignored (technically if the XLIFF file only has a single string, see below for details).  The 'string_id_value' variable is used to look up the translated text that was loaded when TextString.language was assigned a value.

The translate method supports variable substitution, multiple parameters can be passed in:
```ActionScript
TextString.translate(numItems, "string_id_value", 'optional', 'values');
```

The call will use "optional" and "values"  as replacement parameters in the string that's returned.  See below for the XLIFF file format, but the replacements are made using numeric tags in the string, surrounded by curly braces (for example "param {0} and {1}").



getLanguages()
buildConfigFile(callback)
(n, id, …)
language = 'en'

```ActionScript

```
#XLIFF Format
XLIFF (http://en.wikipedia.org/wiki/Xliff) is an open format for translatable content.  There is a lot of flexibility and options in the file that are not used by TextString.  Here's an example of most basic XLIFF file (a description follows):
```XML
<?xml version="1.0" encoding="utf-8"?>
<xliff version="1.1">
    <file original="plurals=2; [0] (default text); [1] (num != 1)" datatype="plaintext" source-language="en" target-language="en">
        <body>
            <trans-unit id="translator [0]">
                <note>This can be your name/email address/twitter handle, whatever!</note>
                <source>Captain Translator</source>
                <target>[Captain Translator]</target>
            </trans-unit>

            <trans-unit id="tap_message [0]">
                <note>{0} is an on-screen item, for example "bank" or "pirate".</note>
                <source>You tapped - {0}</source>
                <target>[You tapped - {0}]</target>
            </trans-unit>

            <trans-unit id="file_count [0]">
                <note>{0} is the number of files that were found.</note>
                <source>There is a file.</source>
                <target>[There is {0} file.]</target>
            </trans-unit>
            <trans-unit id="file_count [1]">
                <note>{0} is the number of files that were found.</note>
                <source>There are {0} files.</source>
                <target>[There are {0} files.]</target>
            </trans-unit>
            <trans-unit id="file_count [2]">
                <note>{0} is the number of files that were found.</note>
                <source>There are {0} files.</source>
                <target>[There are {0} files.]</target>
            </trans-unit>
        </body>
    </file>
</xliff>
```


#File Locations

#Translating
