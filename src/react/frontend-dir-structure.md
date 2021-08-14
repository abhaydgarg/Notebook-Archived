# Frontend directory structure

```text
.
└── src
    ├── assets
    │   ├── scss (Shared/Global SCSS)
    │   ├── images
    │   └── videos
    ├── config
    ├── router
    ├── redux
    ├── sagas
    ├── services
    ├── hooks
    ├── components (Stateless components)
    │   ├── Picker.js (No need to put inside folder, if single file)
    │   └── Button (If have multiple file. index.js just import Button.js and export back)
    │       ├── index.js
    │       ├── Button.js
    │       └── button.scss
    ├── layouts (Stateful and contains all layouts and their components)
    │   ├── shared (dir)
    │   ├── main (Has own stateful views like header, footer etc)
    │   │   ├── Header
    │   │   ├── Footer
    │   │   └── Sidebar
    │   └── Login.js (If no views like header, footer etc)
    └── pages (Stateful components and also contains modals)
        ├── shared (dir) (Contains stateful views shared with pages like TopBar, ActionBar etc)
        ├── ImagePickerModal.js
        └── Post (Page specific stateful views will also be added here)
            ├── PostList
            ├── PostCreate
            ├── PostUpdate
            └── PostShow
```
