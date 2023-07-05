### Displays for OpenRVDAS

OK... basic idea.

One display html/js page.
Loads config from QueryString

```
display.html?Definition_filename
```

display.html loads the definition (json).
Definition looks like:
{
    widget_name: {
        x: as a percentage of container element
        y: as a percentage of container element
        width: as a percentage of container element
        height: as a percentage of container element
        type: <widget type> (each widget in a javascript file)
        options: {
            options defined independantly per widget type
        },
       ...
}

'widget_name': arbitrary text identifier (e.g., 'label for sonar', 'sonar depth')

most widgets will be, by default, transparent (so things like boxes around
groups of display elements don't need to worry about z-order)

Styling will be bootstrap (or bootstrap-ish).

Common options will be:
    color
    bgcolor
    border
    variable
    class
    alarmClass
    disabledClass
 

type:  widget type
Common widget types will be:
    box
    label
    dataval
    guage
    graph
    map

Widget methods to implement:
    _init (not public)
    update:
    edit:
    export: exports json with the current config (after editing)

dataval alarms can have:
    range
    timout
    variable, (or possibly [variables] to fallback on timedout sources)


The main display page will have

Mothership = (function() { })()

Mothership methods will be:
    _init:
        loads the config
        parses the config
        for widgets in config:
            vars = widget.enum_vars()
            for var in vars
                allVars[var] = true
                dispatch[varname] += widget
        inits the websocket
        subscribe to an object with allVars
    run:
        for each var received:
            for widget in dispath[var], append data to an object
        for each object we just created, call corresponding widgt.update
        with the data object.

    stop:
        close the websocket
    onbeforeonload:
        stop()

    
