django-bingo
------------

django-bingo is a bingo app to play bingo games like "bullshit bingo" while listening together to some radio show and marking the words, which were mentioned.

Dependencies
------------

Python packages:

* django 1.5
* django-jquery 1.9.1
* django-colorful 0.1.3
* pytz 2013b
* pillow 2.1.0

Other: a ttf-file as font for the image-export.

Installing
----------

Create a django project. Then add the following options to your settings.py

* append to ```INSTALLED_APPS```
    * 'bingo'
    * 'jquery'
    * 'colorful'
* ```FONT_PATH``` = '/path/to/font.ttf'

Include the bingo-app in your project urls.py, like this:
```url(r'^', include('bingo.urls'))```

You can use the Sites framework to add several different Bingo sites.
Different Sites have different sets of Words, and they will generate a different set of Games, so the Bingos are independend from each other.

Customizing
-----------

Many minor changes can be done with project templates:

* install [django-apptemplates](https://pypi.python.org/pypi/django-apptemplates/) and add it to the ```TEMPLATE_LOADER``` setting.
* add an own templates directory to ```TEMPLATE_DIRS```.
* create custom templates, which inherit from the app templates.

example for "mytemplates/bingo/main.html":

    {% extends "bingo:bingo/base.html %}
    {% block extra_content_top %}Welcome to my bingo game!{% endblock %}

Settings
--------

The game uses the following settings:

### django-bingo settings

#### required

* ```FONT_PATH``` the ttf font used to generate images.

#### optional

* ```BORDER``` size of the field border in the images.
* ```H_BOX_PADDING```, ```V_BOX_PADDING``` padding of the fields in the images.
* ```H_LINE_MARGIN```, ```V_LINE_MARGIN``` margin of the text in the images. Needed to render multiline text nicely.
* ```COLOR_FROM```, ```COLOR_TO``` two integer values. the RGB-values of the randomly generated color for marking fields will be chosen from this range.
* ```NEUTRAL_FIELD_COLOR``` background color of neutral fields
* ```NEUTRAL_WORD_COLOR``` word color of neutral fields
* ```MIDDLE_FIELD_COLOR``` background color of middle fields
* ```MIDDLE_WORD_COLOR``` word color of middle fields
* no ```MARKED_FIELD_COLOR```, because its chosen randomly from ```COLOR_FROM``` to ```COLOR_TO```
* ```MARKED_WORD_COLOR``` word color of marked fields
* no ```VOTED_FIELD_COLOR```, because its chosen randomly from ```COLOR_FROM``` to ```COLOR_TO```
* ```VOTES_WORD_COLOR``` word color of neutral fields
* ```VETO_FIELD_COLOR``` background color of veto fields
* ```VETO_WORD_COLOR``` word color of veto fields
* ```BINGO_IMAGE_DATETIME_FORMAT``` format for the datetime in the board images
* ```FONT_SIZE``` the font size in the images.
* ```GAME_START_DISABLED``` set to ```True```, to disable starting new games.
* ```GAME_START_TIMES``` ```None``` for no restriction or ```((start hour, start minute), (end hour, end minute))``` for restricting the start time to a special time range (i.e. the broadcasting time of the radio show)
* ```GAME_END_TIME``` ```None``` for no restriction or ```(end hour, end minute)``` for setting a time, after which the game is ended. The end time needs to be outside of the ```GAME_START_TIMES``` interval.
* ```GAME_HARD_TIMEOUT``` minutes after the game will be ended, i.e. the duration of the radio show)
* ```GAME_SOFT_TIMEOUT``` minutes of inactivity, after which the game will be be ended.
* ```USER_ACTIVE_TIMEOUT``` minutes after which a user is no longer considered active (number of active users is shown on the bingo page)
* ```SALT``` a salt for hashing the Bingo password hashs. The salt needs to be static, so a BingoBoard can be selected with a query for the hashed password. The users should not use important passwords there, anyway.

### django settings

* ```SITE_ID``` id of the current site in the Sites-Framework

Notes
-----
* At least one of the settings ```GAME_HARD_TIMEOUT``` or ```GAME_SOFT_TIMEOUT``` must be set, even when ```GAME_END_TIME``` is set. When both are ```None```, the game never ends and no new game can be created on the next day.
