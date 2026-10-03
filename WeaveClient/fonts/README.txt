WeaveFonts.swf embeds the fonts listed in WeaveFonts.css. The compiled file is committed as
WeaveClient/swf/WeaveFonts.swf, and `ant build` copies it to ROOT/. Ant does not regenerate it.

After changing WeaveFonts.css, regenerate it from the WeaveClient directory with the Flex 4.5.1 SDK
(the url() paths in the CSS are relative to WeaveClient/, so the CSS has to be compiled from there):

    cp fonts/WeaveFonts.css WeaveFonts.css
    "$FLEX_HOME/bin/mxmlc" -managers=flash.fonts.AFEFontManager -output swf/WeaveFonts.swf WeaveFonts.css
    rm WeaveFonts.css

The result is not byte-identical to the committed SWF, which was originally built with Flash Builder.
