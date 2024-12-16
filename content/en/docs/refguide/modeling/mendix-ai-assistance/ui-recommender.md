---
title: "UI Recommender"
url: /refguide/ui-recommender/
weight: 20
description: "Describes UI Recommender in Mendix Studio Pro."
---

## Using UI Recommender

### Studio Pro 10.18

{{% alert color="info" %}}
 By default, the UI Recommender is enabled. If you prefer to disable it, you can do so in **Edit** > **Preferences** > **Maia** > **In-Editor Recommender** > **Enable for Page Editor**. 
{{% /alert %}}

1. Opening the dialog.

    1. When hovering over an element on the page, a blue indicator (plus/dot) will appear:
        1. The indicator will appear at the closest border of the widget (top/bottom or left/right for inline widgets).
        1. The position of the indicator relates to where an element will be inserted, (for example selecting the indicator at the top or left will insert an element before the current widget).
        1. Click on the indicator to open the dialog:

            {{< figure src="/attachments/refguide/modeling/mendix-ai-assistance/ui-recommender/dot.png" alt="UI Recommender dot" class="no-border" >}}

    2. Alternatively, you can open the dialog using keyboard shortcuts:
        1. Highlight or select an element on the page.
        1. Press <kbd>Ctrl</kbd> + <kbd>,</kbd> on Windows, or <kbd>Command</kbd> + <kbd>,</kbd> on Mac to insert the widget before the element.
        1. Press <kbd>Ctrl</kbd> + <kbd>.</kbd> on Windows, or <kbd>Command</kbd> + <kbd>.</kbd> on Mac to insert the widget after the element.

2. Selecting and inserting the widget

    1. In the dialog, search for the element you would like to add.
    1. Navigate the list using the up or down arrow keys.
    1. Insert the selected element by pressing <kbd>Enter</kbd> or by clicking on the element from the list.
    1. The inserted widget/element will become highlighted/selected, and the dialog will close automatically.
    1. The dialog can also be closed by pressing <kbd>Esc</kbd>.

        {{< figure src="/attachments/refguide/modeling/mendix-ai-assistance/ui-recommender/dialog.png" alt="UI Recommender dialog" class="no-border" >}}

