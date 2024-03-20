# Reporte sobre explicacion del codigo

## Instalacion del proyecto

Se tuvieron algunas cmplicaciones ya que la forma normal que tenia era para instalarlo en istemas operativos diferentes a Windows y eso me causo algunas confuciones y errores, pero los pude solucionar preguntando e intentado varias veces.

### Codigo de crear tareas

```javascript
      function handleSubmit(e) {
    e.preventDefault();

    let todo = document.getElementById('todoAdd').value
    const newTodo = {
      id: new Date().getTime(),
      text: todo.trim(),
      completed: false,
    };
    if (newTodo.text.length > 0 ) {
        setTodos([...todos].concat(newTodo));
    } else {

        alert("Enter Valid Task");
    }
    document.getElementById('todoAdd').value = ""
  }

  <div id="todo-list">
    <h1>Todo List</h1>
        <form onSubmit={handleSubmit}>
            <input
              type="text"
              id = 'todoAdd'
            />
            <button type="submit">Add Todo</button>
        </form>
        {todos.map((todo) =>
            <div className="todo" key={todo.id}>
                <div className="todo-text">{todo.text}</div>
            {/* insert delete button below this line */}
            </div>)}
</div>
```

- En este primero codigo se realizan las funciones y los components necesario para crear la tarea y poderla ver en la pantalla, se observa que en el **JSX** se coloco el metodo onclik en el boton para que se ejecutara la funcion **handleSubmit** al ser presionado y por medio de la funcion **map** se listan todas las tareas que se van creando en la pantalla extrayendolas del estado **todo**.
- La funcion **handleSubmit** se recupera el valor ingrsado en el input y se almacena en el estado **todos** con informacion extra y se valida en caso de que se envie vacio el valor.

### Codigo de eliminar tareas

```javascript
function deleteTodo(id) {
    let updatedTodos = [...todos].filter((todo) => todo.id !== id);
    setTodos(updatedTodos);
  }

  <button onClick={() => deleteTodo(todo.id)}>Delete</button>
```

- Para poder eliminar las tareas se modifica el **JSX** agregando el boton de eliminar tarea en cada tarea que se va creando y cuando es llamado, se le pasa a la funcion el id de la tarea que se eliminara para que desaparezca de la pantalla de forma permanente.
- Despues dentro de la funcion **deleteTodo** se valida que exista en la lista de todas las tareas una tarea con ese **id** y se elimina.

### Codigo de casilla para marcar tareas hechas

```javascript
function toggleComplete(id) {
    let updatedTodos = [...todos].map((todo) => {
      if (todo.id === id) {
        todo.completed = !todo.completed;
      }
      return todo;
    });
    setTodos(updatedTodos);
  }

  <input type="checkbox" id="completed" checked={todo.completed} onChange={() => toggleComplete(todo.id)}/>
```

- Se agrega la casilla para marcar las tareas hechas que es un input tipo checkbox que tiene un metodo onChange que al ser presionado manda el **id** de la tarea en donde se selecciono a la funcion **toggleComplete**.
- Despues dentro de la funcion se realiza una funcion temporal tipo flecha que extrae los datos de **todos**, realiza una validacion para saber si el id que se paso existe en la lista de tareas y si es verdadero se cambia al estado contrario la propiedad **completed**.

### Codigo para modificar

```javascript
 const [todoEditing, setTodoEditing] = useState(null);

 function submitEdits(newtodo) {
    const updatedTodos = [...todos].map((todo) => {
      if (todo.id === newtodo.id) {
        todo.text = document.getElementById(newtodo.id).value;
        }
        return todo;
      });
      setTodos(updatedTodos);
      setTodoEditing(null);
    }
 
 {todo.id === todoEditing ?
        (<input
          type="text"
          id = {todo.id}
          defaultValue={todo.text}
        />) :
        (<div>{todo.text}</div>)
      }
    </div>
    <div className="todo-actions">
      {/* if it is edit mode, allow submit edit, else allow edit */}
      {todo.id === todoEditing ?
      (
        <button onClick={() => submitEdits(todo)}>Submit Edits</button>
      ) :
      (
        <button onClick={() => setTodoEditing(todo.id)}>Edit</button>
      )}
      <button onClick={() => deleteTodo(todo.id)}>Delete</button>
     </div>
  </div>
  ```
  
- Se agrega otro estado para el proyecto que sera el responsable de gestionar los diversos textos que tendra el boton.
- Despues la funciona cuando es ejecutada realiza los cambios en la tarea enviada para cambiar y envia de nuevo un valor null para que cambie el titulo del boton.
- En el **JSX** se realizan diversas validaciones para saber dependiendo el estado que se tenga aparezcan diversos titulos en el mismo boton.

### Codigo para guardar de forma local

```javascript
import React, {useState,useEffect} from "react";

useEffect(() => {
    const json = localStorage.getItem("todos");
    const loadedTodos = JSON.parse(json);
    if (loadedTodos) {
      setTodos(loadedTodos);
    }
  }, []);

useEffect(() => {
    if(todos.length > 0) {
        const json = JSON.stringify(todos);
        localStorage.setItem("todos", json);
    }
  }, [todos]);
```

- Lo que se hace aqui es que se importa de react el **useEffect** que segun a lo que entendí se activa cada vez que haya una recarga o un cambio en el programa y en este caso lo que se hace es almacenar de forma local las tareas que estaran disponibles hasta que se cierre el servidor.
