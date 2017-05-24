### What are Galaxy Webhooks?

A system which can be used to write small JS and/or Python functions to change predefined locations in the Galaxy client
<br><br>
In short: A plugin infrastructure for the Galaxy UI

---

### Entry point:<br>masthead

![](../images/webhook_masthead.png)

At the header menu: Enabling the overlay search, link to communities ...
---

### Entry point:<br>tool/workflow
<br><br>
![](../images/webhook_tool.png)

Shown after tool or workflow execution. Comics, citations, support ...

---

### Entry point:<br>history-menu

![](../images/webhook_history.png)

Adds an entry to the history menu - no functionality as of now

---

### Content of a Webhook:<br>What is it made of?

- a config file in YAML format: `config/NAME.yml`
- optional: A Python helper file with access to the Galaxy `trans` object
  - `helper/__init__.py`
  - provides an API call at `/api/webhooks/WEBHOOK_NAME/get_data`
- optional: Additional `JS` and `CSS` code
  - `static/script.js`
  - `static/styles.css`

---

### Example configuration YAML

```
name: trans_object
type: 
  - masthead
activate: true

icon: fa-user
tooltip: Show Username

function: >
  $.getJSON("/api/webhooks/trans_object/get_data", function(data) {
    alert('Username: ' + data.username);
  });
```

---

### Definition of the configuration options

Argument | Description
--- | ---
`name` | Name of the Webhook (and API call)
`type` | Entry point. `tool/workflow/masthead/history-menu`. More might be available in future
`activate` | (De-)Activates the Webhook. `true/false`
`icon` | Icon to show for a `masthead` plugin. Full list of available icons [here](http://fontawesome.io/icons/)
`tooltip` | Tooltip to show for `masthead` plugins
`function` | JavaScript code to run when `masthead` button is clicked

---

### Example `__init__.py`

```
def main(trans, webhook):
    if trans.user:
        user = trans.user.username
    else:
        user = 'No user is logged in.'
    return {'username': user}
```

The return value can be read with a call to `/api/webhooks/WEBHOOK_NAME/get_data`
---

### Want to integrate Webhooks in your Galaxy instance?

- Copy your Webhook to your Webhook directory on your Galaxy instance
  - default: `config/plugins/webhooks`
  - configurable via `webhooks_dir` in `galaxy.ini`
- Restart Galaxy. While developing, changes at the `__init__.py` file will be active immediately

---

### Want to contribute?

- Create Webhooks and share them with the world!
  - on the [main Galaxy repository](https://github.com/galaxyproject/galaxy)
- Improve the Webhooks implementation
  - enhance existing entry points
  - add additional ones ...
- Improve the documentation or training material
  - [Documentation](https://docs.galaxyproject.org/en/latest/admin/special_topics/webhooks.html)
  - [Galaxy Training Network](http://galaxyproject.github.io/training-material/)

---

### Developing the Webhooks implementation

- Webhooks initialisation: `lib/galaxy/webhooks/__init__.py`
- JavaScript logic: `client/galaxy/scripts/mvc/webhooks.js`
- Entry points
  - `client/galaxy/scripts/mvc/tool/tool-form.js`
  - `client/galaxy/scripts/mvc/tool/tool-form-composite.js`
  - `client/galaxy/scripts/mvc/history/options-menu.js`
  - `client/galaxy/scripts/layout/menu-js`
- API: `lib/galaxy/webapps/galaxy/api/webhooks.py`
  - `lib/galaxy/webapps/galaxy/buildapp.py`

---

### <i class="fa fa-pencil" aria-hidden="true"></i> Hands-on

![](../images/exercise.png)
