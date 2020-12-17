<!--
    This source file is part of the open source project
    ExpressionEngine User Guide (https://github.com/ExpressionEngine/ExpressionEngine-User-Guide)

    @link      https://expressionengine.com/
    @copyright Copyright (c) 2003-2020, Packet Tide, LLC (https://packettide.com)
    @license   https://expressionengine.com/license Licensed under Apache License, Version 2.0
-->

# Emoji

[TOC]

The Emoji module can make it easy for you to provide your visitors with a catalog / click-interface to add expressive emoji to their content. It also enables you to parse emoji code shorthand in content that ExpressionEngine hasn't parsed, such as from an external data source.

## {exp:emoji:emoji_list}

This tag outputs a list of all cataloged emoji:

    <dl>
      {exp:emoji:emoji_list}
        <dt>{short_name}</dt>
        <dd>{html_entity}</dd>
      {/exp:emoji:emoji_list}
    </dl>\1 \2

### Variables

### `{html_entity}`

The HTML entity which will render the emoji.

### `{short_name}`

The short name for the emoji, e.g. _joy_, _man-woman-girl-boy_. Note that this does _not_ include the _:_ before and after the short name that would be necessary for ExpressionEngine to parse and render it for you.

## {exp:emoji:parse_shorthand}

This tag will convert emoji shorthand code (`:robot_face:`) to an HTML entity (`&#x1F916;`) which the browser will render (🤖). ExpressionEngine will automatically parse emoji shorthand codes in content, but this tag enables you to do the same from any content source:

    {exp:emoji:parse_shorthand}
      Flying a :rocket: to Mars with Elon Musk
    {/exp:emoji:parse_shorthand}

    {!-- outputs "Flying a &#x1F680; to Mars with Elon Musk" --}
    {!-- renders as "Flying a 🚀 to Mars with Elon Musk" --}

## Emoji picker example

The door is wide open for how and where you can implement a clickable emoji catalog / autocomplete using the Emoji module. This simple example uses an ajax jQuery UI Dialog, but the basic technique is applicable to any UI or utility you wish to build.

1.  In the template containing your comment form, add a link for the picker and a container for your modal content:

        <a href="" class="emoji-picker-link">Pick Emoji</a>
        <div id="emoji-picker"></div>

2.  And include the following JavaScript (including jQuery, jQuery UI, and the jQuery UI CSS):

        <link rel="stylesheet" href="//code.jquery.com/ui/1.12.1/themes/base/jquery-ui.css">
        <script src="//code.jquery.com/jquery-3.2.1.min.js"></script>
        <script src="//code.jquery.com/ui/1.12.1/jquery-ui.js"></script>
        <script type="text/javascript">
          $(function() {
            $('#emoji-picker').dialog({
              autoOpen: false,
              maxHeight: 300,
            });

            $('.emoji-picker-link').on('click', function() {
              $("#emoji-picker").load("{path='site/emoji-picker'}").dialog('open');
              return false;
            });
            addEmoji = function(emoji) {
              var comment = $('#comment_form textarea[name=comment]');
                comment.val(comment.val() + ' ' + emoji + ' ');
                return false;
            }
          });
        </script>

3.  Create a template called `emoji-picker` (or whatever you like, just match your `{path=}` variable above).
4.  In your new emoji picker template, add the following:

        <style>
          .container{
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(50px, 1fr));
            grid-gap: 1rem
          }
        </style>

        <div class="container">
          {exp:emoji:emoji_list}
            <div class="item"><a href="#" onclick="addEmoji(':{short_name}:');">{html_entity}</a></div>
          {/exp:emoji:emoji_list}
        </div>

5.  🎉💃🕺✨🌐✨

Now visitors can click your emoji picker link, and add emoji to their comments without having to remember 1500+ emoji shorthand codes.
