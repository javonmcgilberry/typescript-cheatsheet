# Typescript Cheatsheet
Typescript

## Implicit Type Checking

`let fruitName = "Banana"; // does not need TS`

ts infers that this is of type string. Any values that are assigned are inferred implicitly to be of a certain type. 


## Explicit Type Checking

When it cannot figure it out you will need to explicitly let ts know what the type is:
`let fruitName: string;`
or
`let fruitName: string =` `'``ddd``'``;`
Now if you try to use fruitName as a number it wont work or will give you an error.

You can define a variable without assigning it but if you try to use it before its assigned ts will show an error. You can bypass with `!:`
`let fruitName!:string;`

**Array types**
`let someArray: string[]`
this will let ts know we are making a variable that is an array of strings.

**Object Type**

    let menu: {
      vegan: boolean; 
      something: string;
      other?: string;
    } = {
      vegan: true,
      something: 'hello'
    }

When using object destructuring the annotation comes afterward the destructuring part, as if the whole destructuring will be one big object being annotated. note that height is optional with `?:` Another example:

    let { vegan, something } : { vegan: boolean, something: string} = menu;

Probably easier to use an interface or a type alias. See below.

**Any type**
any is fallback for when you don't know what type will be used.

**function parameters**

    function something(name: string, age: number, height?: number){}


**fat arrow functions params**
While normally you don't have to wrap the params in () if there is only one, with ts you need to or its a syntax error.

    const something = (name: string) => {}

**function return type**
the actual functions return can be annotated after the ()

    function headsOrTails(): boolean {
      return Math.random() > 0.5;
    }
    
    const headsOrTails = (): boolean => {
      return Math.random() > 0.5;
    };


**Async function return type**
The return of an async function is always a Promise. You can annotate this with Promise<type>
<type> would be the type we are returning. It’s a generic type. More on that below.

    async function getFruitList(): Promise<string[]> {
      const response = await fetch("https://example.com/fruit");
      const fruitList: string[] = await response.json();
      return fruitList;
    }

**Callback function type**

    function mapNumberToNumber(
      list: number[],
      callback: (item: number) => number,
    ) {
      // (parameter) callback: (item: number) => number
      // Implementation goes here
    }

The callback type is a function that expects a number param and returns a number.



## Interface

A way to save the annotation of an object for reuse

    const car: { wheels: number; color: string; electric: boolean } = {
      wheels: 4,
      color: "white",
      electric: true,
    };
    
    //instead
    interface Vehicle {
      wheels: number;
      color: string;
      electric: boolean;
    }
    const car: Vehicle = { wheels: 4, color: "white", electric: true };



## Indexible Type

uses an index signature so an index on the object has no name yet, and we can have many of these on one object.

    interface Fruit {
      [key: string]: string;
      name: string;
    }
    
    
    let apple: Fruit = {
      name: "Apple",
      ripeness: "overripe",
    };


## Type Alias

similar to interface but for any type not just objects. But they cannot use the extends keyword.
a type looks like setting a variable and assigns with the equal sign.

    type item =  string;
## Union type

If something might be one of two types we can create a union type using “|”. This creates a type that makes sure your variable is one thing OR another. You can also combine different type of values in one array with a type of both with () like so:

    type specialarray =  (string | number)[];
    let special: specialarray =  ['ssss', 5]

You might need to check for a property when using it because we don’t know if its the one or the other.
In fact when using optional ?: in an interface, it creates a union type under the hood because it might be undefined.

    interface Fruit {
      name: string;
      sweetness: number;
    }
    interface Vegetable {
      name: string;
      hasSeeds: boolean;
    }
    type EdibleThing = Fruit | Vegetable;
    
    function checkForSeeds(food: EdibleThing) {
      if ("hasSeeds" in food) {
        console.log(food.hasSeeds);
      }
    }


## Intersection types

Similar to union except it creates a type that is one AND another. So make sure your combining to types that work with each other. you cant combine a string and number type obviously. An intersrction uses & instead of |


## Type Literal

when you want a variable to be an exact value, not just a type. And never anything else. Obviously a const is good for this, but sometimes you want an object or a variable that can be ONE of a few things. Type literal helps out with this. See how the type of seasons can only be one of those literal values.

    type Seasons = "spring" | "summer" | "autumn" | "winter";
    
    function seasonMessage(season: Seasons) {
      if (season === "summer") {
        return "The best season.";
      }
      return "It's alright, I guess.";
    }



## enum

good for checking states of things that you never want changed. Better than const of values because a function can still take any value and you might want the param to ONLY be from this set of values in a const. So instead of a const you can use an enum.
**might be better to use literal types with unions instead.**


## Never and void

a function that returns nothing is a return type of “void”. You can set it to never, especially if it never will return or if the function throws an error.



# Generics

Generics represent a type that won't be defined until the type is used in our code.
Sometimes we have a function or class or type that we are not sure what the type being passed will be, but once we do run it and we know, we want to keep the type checking

    function getFirstItem<ItemType>(list: ItemType[]): ItemType {
      return list[0];
    }

With angle brackets we can tell the function to take a type and then make sure we stick with that type for the return and the param or wherever its needed. So this function would work if we passed an array of strings or numbers in. And it will return the same type. No unions or intersections here. Otherwise we would have to do a lot of testing to see what we are returning. This makes sure the type is set properly based on whats passed.

When calling the function we may need to pass params to those angle brackets unless typescript can infer the type implicitly.

Generics are not just for functions

    type Tree<T> = {
      value: T;
      left?: Tree<T>;
      right?: Tree<T>;
    };

The angle brackets are the param for a generic type. It can be used when creating the type or when calling the type to set and constrain it to a type.


# React and Typescript

You will want to also install typescripts definitions for react
`npm install --save-dev @types/react @types/react-dom`

To write JSX you will need to use files that end in .tsx

With new builds you can set tsconfig jsx option to react-jsx or react-jsxdev
This means you no longer need to import React into every file you create

    // tsconfig.json
    {
      "compilerOptions": {
        "jsx": "react-jsx",
        "jsxFragmentFactory": "Fragment"
      }
    }


## React components

React components all have to return either a React Element or `**null**`, so it's usually a good idea to add a return type annotation to your function so TypeScript warns us if we return the wrong thing.

    import { ReactElement } from "react";
    
    function HelloWorld(): ReactElement | null {
      return <div>Hello world!</div>;
    }

You can also use a return of `JSX.Element` which is the same as ReactElement but allows it to be of type any.

Annotating the props

    function Message({ message }: { message: string }): ReactElement {
      return <div>A message: {message}</div>;
    }

You can also make a functional component using the generic type function.

    import { FC } from "react";
    
    const Message: FC<{ message: string }> = ({
      message,
      children,
    }) => {
      return (
        <div>
          A message: {message}
          <br />
          Children: {children}
        </div>
      );
    };



## UseState Hook

The hook usually does not need any type annotation if its being set right away
`const [numberState, setNumberState] = useState(5);`
However if its not set, we need to.
useState is a generic type function so we can pass a param to help it out

    const [stringState, setStringState] = useState<string | null>();

Sometimes you will pass a prop down that is a setState function. This can be annotated like any function props, or make a shape with interface:

    interface UpstreamComponentProps {
      stringState: string;
      setStringState: (newValue: string) => void;
    }

**The issue comes up when setStringState takes in a function for using the old state. For that React Types has a special type you can use:**
`setStringState: Dispatch<SetStateAction<string>>;`


## UseReducer

A function that returns a state which is usually an object, and a dispathc function which will call your reducer function and expects an action to be passed. Its good to make some interfaces first:

Here you can see we made some types and interfaces. 

    type Category = "Bread" | "Fruit" | "Vegetable" | "Meat" | "Milk";
    
    interface ShoppingListItem {
      id: string;
      title: string;
      completed: boolean;
      category: Category;
    }
    
    // the reducer state object. An array of objects of shopping list items
    type ShoppingListState = ShoppingListItem[];
    
    //the actions that will be passed to the reducer function
    interface AddAction {
      type: "add";
      category: Category;
    }
    interface EditAction {
      type: "edit";
      id: string;
      title: string;
    }
    interface DeleteAction {
      type: "delete";
      id: string;
    }
    interface CompleteAction {
      type: "complete";
      id: string;
    }
    
    // the shopping list action literals. it must be one of these actions
    export type ShoppingListAction =
      | AddAction
      | EditAction
      | DeleteAction
      | CompleteAction;

And finally the reducer function:
You will want to make an interface for all action types that will be passed tot he dispatch() which in turn will call your reducre with the current state for you. Aboe we make shoppingListAction. It points to each type of action we will pass. Usually the action has a state of “loading” or error etc…
It also has the new data that will perhaps be added to state.
Also make sure to create a state interface. and if state is holding many objects, make a type that holds an array of that state.

then:



    function shoppingReducer(
      state: ShoppingListState,
      action: ShoppingListAction,
    ): ShoppingListState {
      switch (action.type) {
        case "add":
          return state.concat({
            // Good enough random ID generator.
            id: `${Math.random()}-${Date.now()}`,
            title: "",
            category: action.category,
            completed: false,
          });
        case "complete":
          return state.map((item) => {
            if (item.id === action.id) {
              return { ...item, completed: true };
            }
            return item;
          });
        case "delete":
          return state.filter((item) => item.id !== action.id);
        case "edit":
          return state.map((item) => {
            if (item.id === action.id) {
              return { ...item, title: action.title };
            }
            return item;
          });
        default:
          return state;
      }
    }

Now when we call our reducer, typescript can help us with all the definitions!
`const [state, dispatch] = useReducer(shoppingReducer, []);`

See:
https://codesandbox.io/s/practice-typescript-usereducer-forked-kgc65


## Event Handlers

for events you can hover on the onClick or onChange event and get a hint and use that for your parameter as the first example below. Or you can use the built in types for react and set the function itself to a certain type. Doing that will not even require a return type, like void.

    function handleOnChange(
      event: React.FormEvent<HTMLInputElement>,
    ): void {
      // ...
    }
    const handleOnChange: ChangeEventHandler<HTMLInputElement> = (
      event,
    ) => {
      // ...
    };



## Using “As”

In some cases, we might want to use the same event handler with two different kinds of elements, such as buttons and anchor tags. In this case, it might make more sense to assert that our target is a union of the two element types. then when you are checking evet.target for example you may check it “as” a button or an anchor which have different return values.


    function handleClick(
      event: React.MouseEvent<HTMLButtonElement | HTMLAnchorElement>,
    ) {
      const target = event.target as
        | HTMLButtonElement
        | HTMLAnchorElement;
    
      // If target has an `href` property, we know it's an anchor
      if ("href" in target) {
        target.href;
      }
    }



## Children Prop

The children prop will either be a function or a ReactNode type

