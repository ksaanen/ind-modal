# ind-modal

Lightweight implementation of modal dialogs/panels for Angular.

## Demo
![Demo](/images/demo.gif)

## Installation

To install this library, run:

```bash
$ npm install ind-modal --save
```

## Usage

Import the `IndModalModule`:

```ts
import { IndModalModule } from 'ind-modal';
...

@NgModule({
  declarations: [
    ...
  ],
  imports: [
    ...,

    // Import this module in order to use the "ind-modal" component and ModalService
    IndModalModule
  ],
  providers: [
    ...
  ],
  bootstrap: [
    ...
  ],
  entryComponents: [
    // Add here modal components that you want to open using ModalService
  ]
})
export class AppModule { }

```

### Option 1 - Inside a template of another component
Define your modal dialog inside a template of another component. For example, in your `home.component.html` you could have:

```html
<ind-modal #firstModal>
  <ind-modal-header>
    I am first modal
  </ind-modal-header>
  <ind-modal-content>
    This modal has its own header, content and footer.
  </ind-modal-content>
  <ind-modal-footer>
    <button (click)="firstModal.close()">okay!</button>
  </ind-modal-footer>
</ind-modal>
```
Then open the dialog as follows:

```html
<button (click)="firstModal.open()">Simple Dialog</button>
```

### Option 2 - Separate component + ModalService
Define your modal dialog as a separate component.

**modal.component.html**
```html
<ind-modal #modal>
  <ind-modal-header>
    I am a modal component
  </ind-modal-header>
  <ind-modal-content>
    I am opened programmatically from code.
  </ind-modal-content>
  <ind-modal-footer>
    <button (click)="modal.close()">okay!</button>
  </ind-modal-footer>
</ind-modal>
```

**modal.component.ts**
```ts
import { Component } from '@angular/core';
import { ModalComponentBase } from 'ind-modal';

@Component({
  templateUrl: './modal.component.html'
})
export class ModalComponent extends ModalComponentBase {
}

```
Note that the component has to extend `ModalComponentBase`.

**Important**: Make sure you add your component to `entryComponents` inside `@NgModule` declaration:

```ts
@NgModule({
  declarations: [...],
  imports: [
    ...,
    IndModalModule
  ],
  providers: [...],
  bootstrap: [...],
  entryComponents: [
    ModalComponent
  ]
})
```
Inject the `ModalService` where you want to open your modal component from. For example:

**home.component.ts**
```ts
import { Component } from '@angular/core';
import { ModalService } from 'ind-modal';
import { ModalComponent } from './modal.component';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html'  
})
export class HomeComponent {
  constructor(private modalService: ModalService) {    
  }

  openModal() {
    this.modalService.open(ModalComponent);
  }
}

```

### Option 3 - Router
Define your modal dialog/panel as a separate component, like in *Option 2*, and open it by navigating to a route.

**modal-route.component.html**
```html
<ind-modal type="side-panel" [routeBehavior]="true">
  <ind-modal-header>
    I am a side panel within a route
  </ind-modal-header>
  <ind-modal-content>
    This the content of the side panel.
  </ind-modal-content>
</ind-modal>

```
Note here `[routeBehavior]="true"`. 

**modal-route.component.ts**
```ts
import { Component } from '@angular/core';

@Component({
  templateUrl: './modal-route.component.html'  
})
export class ModalRouteComponent {
}

```
Nothing special needs to be defined in the component class (no `ModalComponentBase` required like in **Option 2**).

Define a route:

**app.routing.module**
```ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes, PreloadAllModules } from '@angular/router';

import { ModalRouteComponent } from './modal-route.component';
import { HomeComponent } from './home.component';

const routes: Routes = [
  { 
    path: '', component: HomeComponent, children: [
      { path: 'modal-component', component: ModalRouteComponent }
    ]
  }
];

@NgModule({
  imports: [
    RouterModule.forRoot(routes)
  ],
  exports: [
    RouterModule
  ]
})
export class AppRoutingModule { }
```

Open the panel by navigating to `/modal-component`:

```html
<button [routerLink]="['modal-component']">child route modal component</button>
```

### Playground App

See the above examples in action by running the Angular app in the **playground** folder. 

In order to run the example application you need to first build the library and link it to **node_modules** in the **playground** folder. Please follow the following steps:

```bash
# 1. Install dependencies
yarn install

# 2. Build the library
npm run build

# 3. Go to /dist folder
cd dist

# 4.  Create an NPM link
npm link

# 4. Go to the "playground" folder
cd ../playground

# 5. Install dependencies
yarn install

# 6. Run the example application
yarn start
```

Now you can navigate to `http://localhost:4200` to use the **playground** app.

## Styles

The library comes with a predefined set of styles, but you're free to style the modal dialogs/panels the way you want. In order to include default styles in your Angular CLI application, add the following to the `styles` section of `.angular-cli.json`:

```json
"styles": [
  ...,
  "../node_modules/ind-modal/styles.css"
],
```

If you would like to customize the styles, you can copy the SASS source code from `node_modules/ind-modal/styles.scss` to your application and change it the way you need.

## Development of the Library

To generate all all the package assets in the `dist` folder run:

```bash
$ npm run build
```

To lint all `*.ts` files:

```bash
$ npm run lint
```

To test the library with the `playground` application link the `dist` folder using `npm link`:

```bash
# In "dist" folder
npm link

# in "playground" folder. This step is also done automatically after "yarn install"
npm link ind-modal
```

## License

MIT © [Independer.nl](https://werkenbij.independer.nl/)
