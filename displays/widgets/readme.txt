Create a Widget class ??  Or just a prototype?

Widget init will:
    create the div, parse basic CSS
    Can hold the '_extend' method, and any other method common to many 
    widgets (export comes to mind).
    Will hold 'console.log(not implemented)' functions for
        edit, update, 
    

WidgetInstance init will:
    call super(conf)
    set default options, etc

    
Somewhere in here we want to get status:logger_status[logger_id].status
If status is "EXITED", we gray out.

class Widget {

    constructor(conf) {
        parseCSS(conf); // returns an object with CSS property/values
    }

    static parseCSS(config) {
        var css = {};
        // parse conf and turn x,y, width, height into CSS
        // merge conf.style, default, style,  and parsed values
        // set this.style
    }
    extend() {
       // put the extend code here
    }
 
    createDiv() {
        this.div = document.createElement('div');
        // apply this.style to it
    }

    export() {
        return this.config || {};
    }

    edit() {
        console.warn("edit method not implemented");
    }

    update() {
        console.warn("update method not implemented");
    }
}

class Text extends Widget {
    static def_opts = {
        format: '%s',
    }

    constuctor(conf) {
        super(conf);
        var conf_opts = conf.options || {};
        conf.options = extend(def_opts, conf_opts);
        this.conf = conf;
        // Everything above is pretty much boiler plate.
        // Here we get Text specific stuff
        opts = conf.options;
        if (opts.timeout) {
            this.timer = setTimout(opt.timeout * 1000, timed_out);
        }
    }

    update(data) {
        // Might need to check more stuff, because it would be nice
        // to know if the logger for this data is enabled, and if
        // off then we gray out or something.
        // probably just want the last value
        (time, value) = data.pop();
        this.last_update = time
        this.latest_data = data;
        if (this.timer) {
            // Although we could have a range problem, and 
            // we don't want to flicker, so....
            GoodData();
            clearTimeout(this.timer);
            this.timer = setTimout(opt.timeout * 1000, BadData);
        }
        // Bounds checking
        if (this.conf.options.range) {
            range = this.conf.options.range;
            if (value > range.min && value < range.max) {
                GoodData();
                this.div.classList.remove('BadDog');
            } else {
                BadData();this.div.classList.add('BadDog');
            }
        }
        this.div.innerHTML = value;
    }

    static GoodData() {
        //for class in this.conf.options.BadDog
        //    this.div.classList.remove(class)
        //for class in this.conf.options.GoodDog
        //    this.div.classList.add(class)
    }
    static BadData() {
        //for class in this.conf.options.GoodDog
        //    this.div.classList.remove(class)
        //for class in this.conf.options.BadDog
        //    this.div.classList.add(class)
    }

    static timed_out() {
        BadData();
    }
    static inRange(x) {
        return (x > this.min && x < this.max);
    }
}
