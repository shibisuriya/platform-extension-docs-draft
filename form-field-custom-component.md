---
id: form-field-custom-component-draft
aliases: []
tags: []
---

# Custom components that targets Kissflow's form fields

## Prerequisites

- If you are on a mac, then install homebrew.

- Use homebrew to install nvm.

- Install node v18.18.0 (or any other supported version
  using nvm).

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

- Some modules are mandatory (for example, the project which
  targets kissflow's form field, the `web/FormField.jsx` is
  mandatory).
