### What is OpenRVDAS
Read up on the parent project [here](https://github.com/OceanDataTools/openrvdas)

### OpenRVDAS Sqlite GUI
OK... if you've arrived here, you're out on the bleeding edge.

This is an early pass at a Django-free, pure-SQLite+Javascript GUI for OpenRVDAS. To use it,
check this repository out in a separate directory, e.g.

    /opt/oceandatatools/sqlite_gui

Then create a symlink to it from the OpenRVDAS root directory.

    cd /opt/openrvdas
    ln -s /opt/oceandatatools/sqlite_gui

Then run the SQLite GUI installation script.

    cd sqlite_gui
    utils/install_sqlite_gui.sh

Once that has completed, you'll need to create a user for the SQLite web interface:

    cgi-bin/user_tool.py -add --user <user> --password <password>

Eventually the installation script will reload the supervisor daemon and set the things 
you want running, but for now, do it manually:

    supervisorctl reload  # then wait a few seconds
    supervisorctl stop openrvdas:* nginx:*  # stop the Django-based scripts
    supervisorctl start sqlite:*  # start sqlite-based scriptw
    supervisorctl status  # check that everything is running as intended

At this point, goint to https://hostname should take you to the initial (still
rudimentary) SQLite GUI page.
