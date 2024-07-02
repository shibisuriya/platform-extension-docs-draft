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

- Run the command `npx @shibi-snowball/c3`

- The scaffolder will ask for the project's name and the
  project's target (category).

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

# Form runtime

## Validations

- If you don't supply a `custom error message` while adding
  a validation for a custom field in the form wizard,
  Kissflow's default error message for that particular error
  will be supplied to the custom component via props api.
