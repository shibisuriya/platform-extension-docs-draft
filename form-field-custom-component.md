---
id: form-field-custom-component-draft
aliases: []
tags: []
---

# Custom components that targets Kissflow's form fields

## Prerequisites

### Installing homebrew if you are using a mac

- If you are on a mac, then install homebrew.

  - If you are using zsh which is the default shell of mac
    then source homebrew in ~/.zshrc file.

  - Source homebrew in ~/.zshrc file

  ```bash
      export PATH="/opt/homebrew/bin:$PATH" # for homebrew.
  ```

  - If you are using bash then this part is not for you, you
    set it up your self, go to
    [#Scaffolding a project](#scaffolding-a-project)(scaffolding
    a project) section.

### Installing nvm

- Use homebrew to install nvm.

  - Add nvm related details to `.zshrc`

  ```bash
    export NVM_DIR=~/.nvm
    source $(brew --prefix nvm)/nvm.sh
  ```

- Install node v18.18.0 (or any other supported version
  using nvm). The verison of node is very important,
  arbitary version of node is not fine, because eslint won't
  work, both on vscode's diagonistics, in the webpack and
  from the cli.

- Select the appropriate node version from nvm.

## Scaffolding a project

- Run the command `npx @shibi-snowball/c3@latest`

  Note: Don't miss the '@latest' in the command, obiviously
  the command will work without the it but the latest
  published version of the scaffolder won't kick in.

- The scaffolder will ask for the project's name and the
  project's target (category).

- Note that the page based custom components (the apps team
  basically), have a scaffolder of their own, this
  scaffolder will be merged with c3. And we can scaffold a
  vite base app that targets kissflow's page + lcnc/sdk
  preinstalled and configured in that react / vanilla app.

- For now 'git init && git add . && git commit -m 'initial
  commit'` will create a commit, but I will try to implement
  this to be done automagically.

- Ones scaffolded, cd into the folder and issue
  `npm install`.

- To serve the project for development, run `npm run dev`.

- To build the project for distribution, run
  `npm run build`.

- To zip the dist for uploading into the platform,
  `npm run zip`

### `The c3.config.js` file

```js
import {
  C3_COMPONENTS,
  PLATFORMS,
  PROJECT_TARGETS,
} from "@shibi-snowball/c3-model";

export default {
  category: PROJECT_TARGETS.FORMS,
  components: {
    [PLATFORMS.WEB]: {
      [C3_COMPONENTS.EDITABLE_TABLE]:
        "src/web/EditableTable.jsx",
      [C3_COMPONENTS.FORM_FIELD]: "src/web/FormField.jsx",
      [C3_COMPONENTS.READONLY_TABLE]:
        "src/web/ReadonlyTable.jsx",
      [C3_COMPONENTS.CARD]: "src/web/Card.jsx",
    },
    [PLATFORMS.PWA]: {
      [C3_COMPONENTS.FORM_FIELD]: "./src/pwa/FormField.jsx",
      [C3_COMPONENTS.READONLY_TABLE]:
        "./src/pwa/ReadonlyTable.jsx",
      [C3_COMPONENTS.CARD]: "./src/pwa/Card.jsx",
    },
  },
};
```

- A javascript file named`c3.config.js` is used for
  configuring the project.

- Every module (component) corresponds to touchpoint in the
  platform.

- Every entry in the components key must export a `default`,
  the default export must be React component. Named exports
  won't work.

- Some modules are mandatory (for example, the project which
  targets kissflow's form field, the `web/FormField.jsx` is
  mandatory). If you miss this then the build is rekt, the
  project won't build.

# Dev environment

- If you are using vscode to edit the project, update your
  vscode to the latest version. Latest version of vs code is
  required for eslint's lsp to work on a c3 project.

# Form wizard

- The custom fields ones dropped into the wizard, won't be
  interactable.

# Input params

While publishing a c3 project we shall have a key called
[[publishing-a-custom-component-project#creating-a-custom-field|"property"]]
in the json where the info. about the input param will be
stored. Input params will have a default value which could
be changed per form field while setting it up in the wizard,
this data will be stored per form field in its metadata
node. The problem is we have provision to change input param
and republish the c3 project, if this happens, the data
saved in the nodes become obsolete, because the input param
could have be removed or modified. **Versioning will be
used.** I don't know what 'versioning' means. #partha #jay
#followup

# Form runtime

## Validations

- If you don't supply a `custom error message` while adding
  a validation for a custom field in the form wizard,
  Kissflow's default error message for that particular error
  will be supplied to the custom component via props api.

## The api design

- Subjected to change

```js
const props = {
  cell: {
    focused: true | false,
  },

  value: "", // the current value of the field.
  field: {
    id: "", // the id of the field, id is providied by the kissflow platform
    name: "", // the name of the field, set in the form wizard.
    type: "", // the field's type, text, phone number field, custom smiley, etc.
    isRequired: "", // this is set in the wizard as well.
    hint: "", // this is set in the wizard as well (the help text).
    defaultValue: "", // The defaault value is set in the form wizard as well.
    decimalpoints: "", // Number of decimal points for a number base custom field.
    isComputed: true, // If the value is computed, then just recieve the value via prop, don't try to update the value using `updateValue` and expect it to change.
    color: "", // color is configured in the style tab of the wizard, the final color will be given to the custom component. framework will handle color precendece (based on rules)...
  },
  actions: {
    // Copied the word 'actions' from  **redux**.
    updateValue, // Update the 'model', don't call the http api to update the db yet.
    validateField, // don't know it's purpose.
  },
  parameter: {}, // the input params that are configured will be listed here.
  readonly: true, // if the field is in read only state.
  disabled: true, // same as above for disabled state.
  errors: ["", ""], // a list of errors are the validations set in the wizard is run.
  theme: "dark", // | "light" | "dim", the platform current theme applied by the user.
  fetchKfApi, // this is for fetching data from our platform, like dataset to be used in the dropdown or something.
};
```

- Each touch point will have a varition of this, for
  example, editable table will only have the `cell` property
  will in its api (props), remaining touch points wont' have
  it.

  - Write api docs for each touch point separately, or
    mention that there is an extra property (or some
    properties are removed in a specific case).

# Form wizard

## Form wizard preview

- In form wizard all fields have 'readOnly' set to true by
  Kissflow's framework.
  ![form-wizard-readonly.png](assets/imgs/form-wizard-readonly.png)
