rhelmot's Celeste Magic Timer
=============================

This is an autosplitter for Celeste on Linux, working on all versions and also with Everest installed. I guess it could theoretically be made to work on other games but why would you do that?

The Tracer
----------

The first component of the setup is the tracer. This is a program which reads Celeste's memory and dumps out information about the game state to a file. You can build it by typing `make` in the `tracer` folder.

The tracer can operate in two modes - one where it launches Celeste itself and thus does not require elevated priveleges, and one where you launch it as root and it attaches to the Celeste process. These, along with the path where you want the autosplitter data dumped, are specified on the command line.

The way I've set it up is to have a loop always running in the background which is constantly trying to connect to any running Celeste instances. In order to do this securely, I've put the relevant paths in `celeste_tracer_loop_mine.c` and compiled it as a setuid binary - this means that it can be easily run with root priveleges on login if you changes the paths in the C source to work correctly on your computer.

### How does it work?

Crawling the mono data structures. It's amazing; please don't ask.

The Timer
---------

The `timer` folder contains python scripts that read the autospliter info file and track splits. The most basic file is `celeste_timer.py`, which simply formats the data to text on the screen. This is useful for verifying that the tracer is working. This file is also a library which provides to the other scripts the abiliy to access this data, and also some primitives for manipulating splits. There is a convention that the autosplitter data should be stored in `autosplitterinfo` in the root of this repository.

The next-most important script is `full_splits.py`. This is a standard autosplitter program. It takes as input a path to a route file (a pickle dump which contains a `celeste_timer.Route` object), and tracks your pb and gold splits. It uses the convention that routes should be stored in `timer_data/<name>.route`, pb data should be stored in `timer_data/<name>.pb`, and gold split data should be stored in `timer_data/<name>.best`. The timer will show you desktop notifications for split status and has keyboard shortcuts for resetting and skipping forward and backwards.

The next-most important scripts are the `make_*_splits.py` files. These are programs which interactively construct a `.route` file for you given some common templates. There is a lot of additional power you can unlock by manipulating the routes manually in the python shell, for example, subsplits.

Finally, we hae `stream.py`, which is another autosplitter program which formats its data in a stream-friendly format. This one has much better coding standards, and should be used as a base if you want to write your own display program.

Tips
----

This project was made for myself over the course of various long nights of intensive hacking. The code is quite messy and unreadable in places. However, it should all be totally hackable if you know python.

I've provided a sample .route file, anypercent.route, which is the any% route I use. It uses most of the interesting features, if you want to dive into it.

Wishlist
--------

These are a handful of things which are totally doable but I just don't particularly care to get around to them for now:

- Add current spawnpoint to tracer dump. This will allow you to split on summit flags.
- Dumping current level SID to allow for timing custom maps
- A livesplit-like program which allows in-depth editing of splits
- "Best possible time" feature
