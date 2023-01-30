<!--Note: Source code from tutorial by Diego Salinas Gardón. Acknowledgements and Citations in README.md file contains exact links.-->

<script>
	// BELOW: #Component: Icons are being imported from "Icon.svelte" file in src
	import Icon from "./Icon.svelte";
	let newItem = "";
	// BELOW: #Reactive, array will be updated every time a new TODO is added (via the add() function below) or removed (via remove() function below)
	let todoList = [];

	// BELOW: #Reactive, will be updated every time add() or remove() functions are called (Added by Sam)
	let number = 0;

	// BELOW: #Component: The reusable function "add" is called every time a TODO is added by presing the "+" button
	function add() {

	  // BELOW: #controlFlow: Conditional statement ensuring new TODO is not blank before making it an object and adding it to todoList 
	  // ALSO BELOW: #Reactive: If text box text is not empty, it is passed to a TODO item in ToDoList, which is updated in real time, and the text box attribut NewItem is set back to blank
	  if (newItem !== "") {
		todoList = [
		  ...todoList,
		  {
			task: newItem,
			completed: false,
		  },
		];
		newItem = "";
		// BELOW: #Reactive, updates TODO item count in real time (Added by Sam)
		number++;
	  }
	}
	
	// BELOW: #Component: The reusable function "complete" is called every time the check mark button of a TODO object is pressed
	// BELOW: #Reactive #Properties: By using the "completed" propery if newItem, the program can tell whether to cross out the text or not, this is done by calling the complete() function
	function complete(index) {
	  todoList[index].completed = !todoList[index].completed;
	}

	// BELOW: #Component: The reusable function "remove" is called every time the delete button is pressed next to a TODO. 
	function remove(index) {
	  // BELOW: #Reactive: specified object (at passed in index) in todoList array is deleted, and array contents is updated in real time on display 
	  todoList.splice(index, 1);
	  // BELOW: #Reactive, updates TODO item count in real time (Added by Sam)
	  number--;
	  todoList = todoList;
	}
  </script>
  
  <main>
	<!-- Header -->
	<!--BELOW: #Properties:  h1 header inherits CSS style from the <style> portion of this doc (lower down)-->
	<h1>My to-do list</h1>
	
	<!--BELOW: #Reactive, updates TODO item count in real time (Added by Sam)-->
	<!--ALSO BELOW: #Properties:  h1 header inherits CSS style from the <style> portion of this doc (lower down)-->
	<h2>You have {number} to-do's</h2>
  
	<!-- Form -->
	<form on:submit|preventDefault={add}>
	  <input bind:value={newItem} placeholder="Enter to-do" />
	  
	  <!--BELOW:  -->
	  <button class="add-todo" on:click={add}> <span>+</span></button>
	</form>
  
	<!-- To-dos -->
	<div class="todos">
	  {#each todoList as item, index}

	  <!--Below: #Properties: every todo object will have the same CSS style as default, inherited from CSS lower down in <style> section of file*/-->
		<div class="todo" class:completed={item.completed}>
		  <span class="todo__text">{item.task}</span>
		  <div class="todo__buttons">
			
			<!-- BELOW: #Component: Icons "check-mark" and "delete", including their file paths, are "reused" from the Icon.svelte file to be button icons-->
			<button class="complete" on:click={() => complete(index)}>
			  <Icon name="check-mark" />
			</button>
			<button class="delete" on:click={() => remove(index)}>
			  <Icon name="delete" />
			</button>

		  </div>
		</div>
	  {/each}
	</div>
  </main>
  
  <style>
	main {
	  display: flex;
	  flex-direction: column;
	  align-items: center;
	  min-height: 100%;
	  padding: 5vmin;
	  box-sizing: border-box;
	  background: antiquewhite;
	}
	/**Below: #Properties: every h1 header will have the below-specified CSS style as a default*/
	h1 {
	  text-align: center;
	  font-size: 1.5rem;
	  margin: 2em 0;
	}
	form {
	  width: 100%;
	  max-width: 500px;
	  display: flex;
	  align-items: center;
	  margin-bottom: 1rem;
	}
	input {
	  flex-grow: 1;
	  width: 0;
	  border: none;
	  border-bottom: 1px solid black;
	  background: transparent;
	  box-shadow: none;
	  font-size: 1.2rem;
	  margin: 0;
	  outline: none;
	}
	button.add-todo {
	  width: 2rem;
	  height: 2rem;
	  margin: 0;
	  background: transparent;
	  border: 1px solid black;
	  border-radius: 100%;
	  flex-shrink: 0;
	  margin-left: 1rem;
	}
	button.add-todo span {
	  display: flex;
	  align-items: center;
	  justify-content: center;
	  line-height: 0;
	}
	.todos {
	  width: 100%;
	  max-width: 500px;
	}
	/*Below: #Properties: every todo object will have the same CSS style as default*/
	.todo {
	  display: flex;
	  padding: 20px;
	  border-radius: 20px;
	  box-shadow: 0 0 15px rgb(0 0 0 / 20%);
	  background-color: hsla(0, 0%, 100%, 0.2);
	  margin-top: 1rem;
	  font-size: 1.2rem;
	  justify-content: space-between;
	  align-items: center;
	}
	.todo__buttons {
	  display: flex;
	  align-items: center;
	  margin-left: 1rem;
	}
	.todo button {
	  width: 32px;
	  height: 32px;
	  padding: 4px;
	  margin: 0;
	  flex-shrink: 0;
	}
	.todo.completed {
	  color: slategray;
	}
	.todo.completed .todo__text {
	  text-decoration: line-through;
	}
	.todo.completed button {
	  color: silver;
	}
	button.delete,
	button.delete:hover {
	  color: brown;
	  transition: color 100ms ease-out;
	}
	button.complete,
	button.complete:hover {
	  color: cadetblue;
	  transition: color 100ms ease-out;
	}
  </style>