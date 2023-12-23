Welcome to the **Handy Console** documentation page. Here you will find all the information for the tool.

{:toc}

## Get Started
In order to get started as quickly as possible, simply do the following steps:

1. Ensure you have Text Mesh Pro package installed (this is done by default for new projects)
2. Make sure you have an Event System in your scene (navigate to ```GameObject > UI > EventSystem``` to add one)
3. Navigate to ```Assets/Handy Console/Prefabs``` and drag the ```Console``` prefab to your scene.

After this, Handy Console should be all set and ready to use!

## Creating Commands

When using Handy Console, you will probably want to create your custom commands to fit your needs. To do this, you need to add the ```Command``` tag. For example:

```C#
using HandyConsole;
...
[Command("add", description="A command that adds two numbers together and returns the result.")]
public static int Add(int a, int b) {
    return a + b;
}
```

You can call this command in the console by typing:

```cmd
/add 1 2
```

The ```[Command]``` attribute takes 3 arguments:
1. ```command```: A string that ...gives a name to the command...
2. ```usage```: Optional. You can specify how the manual command will show the usage of the command. By default, this is automatically generated based on the parameters in the function.
3. ```description```: Oprional. The description of the command that the manual command will show. By default, the value is an empty string.

## Argument parsing
### Simple types
Passing simple types as command arguments is straightforward: simply type the argument and the console will parse it. Refer to the ```/add``` command example from above.

### Strings
Strings are a bit different. If the string doesn't contain any white spaces, you can type it without quotes. On the other hand, if it contains white spaces, it needs to be put in quotes. In case you need to use a quote inside a string parameter, put the parameter in quotes and simply type the quote sign as part of the string.

For example, if we have the command ```/hi``` that takes a string as an argument, you can pass the string in the following ways:

```
/hi John
/hi "John"
/hi ""John""
```

In the first two cases, the passed argument will have the value ```John```, while in the third case, it will have the value ```"John"```.

### Complex types
In order to parse a complex type as an argument, you need to put it in braces. Inside these braces you need to give arguments that correspond to one of the constructors of the object. For example, let's take a look at the ```/teleport``` command, which takes a string as its first argument and a ```Vector3``` as its second argument:

```cmd
/teleport "Game Object" (0,0,0)
```

You can also use complex types within complex types. Imagine the scenario:

```C#
public class Position {
    public Position(Vector3 position) { ... }
}

public class Commands {
    [Command("set_position")]
    public static void SetPosition(Position position) { ... }
}
```

You can call the ```set_position``` function by typing:

```cmd
/set_position ((0,0,0))
```

## Macros
Macros are user-defined words that are replaced by a value that they represent. 

To define a macro, use the command ```/add-macro```:

```
/add-macro identity (0,0,0,1)
```

To use a macro, type the ```#``` sign, followed by a previously-defined word:

```
/ instantiate "Game Object" (0,0,0) #identity
```

To remove a previously-defined macro, use the command ```/remove-macro```:

```
/remove-macro identity
```

## OOTB Commands
Handy Console comes with a couple of builtin commands:

### Base commands

| Command | Description |
| --- | --- |
| ```/help``` | Prints information about Handy Console and how to use it. |
| ```/man``` | Shows the description and the usage of the specified command. |
| ```/list-commands``` | Shows a list of all possible commands. |
| ```/count-commands``` | Shows the number of loaded commands. |
| ```/clear``` | Clears the whole output of the terminal. |
| ```/add-macro``` | Defines a special word that can be used to replace the given value. To use the defined word, type ```#[word]```, and it will be replaced by its value. |
| ```/remove-macro``` | Removes a previously-defined special word. |
| ```/macros``` | Shows a list of all possible macros on the terminal. |

### Util commands

| Command | Description |
| --- | --- |
| ```/instantiate``` | Instantiates a game object prefab from the ```Resources``` folder with the specified name at the specified position with the specified rotation. |
| ```/destroy``` | Destroys the first found game object with the specified name in the current active scene. |
| ```/teleport``` | Sets the position of the specified game object to the new position. |
| ```/translate``` | Translates the specified game object with the specified amount. |
| ```/set-rotation``` | Sets the rotation the specified game object to the specified eauler angles. |
| ```/rotate``` | Rotates the specified game object with the specified amount. |
| ```/set-active``` | Activates or deactivates the specified game object. |
| ```/add-component``` | Adds the specified component to the specified game object. |
