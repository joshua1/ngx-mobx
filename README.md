[![Build Status](https://travis-ci.org/NetanelBasal/ngx-mobx.svg?branch=master)](https://travis-ci.org/NetanelBasal/ngx-mobx)
[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/)
[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg?style=flat-square)](https://github.com/semantic-release/semantic-release)
[![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)

# Mobx decorators for Angular Applications ([live example](https://stackblitz.com/edit/angular-mobx-netanel))

## Installation
```js
npm install ngx-mobx
yarn add ngx-mobx
```

## Usage

- `fromMobx` - RxJS bridge from Mobx `computed` function
```ts
class TodosStore {
  @observable todos: Todo[] = [new Todo('One')];
  
  @action addTodo(todo: Todo) {
    this.todos = [...this.todos, todo];
  }
}

@Component({
  selector: 'app-todos-page',
  template: `
   <button (click)="addTodo()">Add todo</button> 
   <app-todos [todos]="todos | async"   
              (complete)="complete($event)">
    </app-todos>
  `
})
export class TodosPageComponent {
  todos : Observable<Todo[]>;

  constructor( private _todosStore: TodosStore ) {}
  
  ngOnInit() {
                                                        // true by default
    this.todos = fromMobx(() => this._todosStore.todos, invokeImmediately);
  }

  addTodo() {
    this._todosStore.addTodo({ title: `Todo ${makeid()}` });
  }
}
```

- `CleanAutorun with autorun` - autorun decorator which cleans itself as soon as the component is destroyed

```ts
import { CleanAutorun, autorun } from 'ngx-mobx';

@CleanAutorun
@Component({
  selector: 'todos',
  template: `...`
})
export class TodosPageComponent {

  @autorun
  ngOnInit() {
    this.todos = this.todosStore.todos;
  }

  ngOnDestroy() {}
}
```

License
----

MIT
