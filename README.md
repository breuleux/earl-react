
earl-react
==========

`earl-react` defines a set of macros to ease development with the
React framework.

What follows is the todo list example from React's homepage, rewritten
with [Earl Grey](http://earl-grey.io) and earl-react:

`script.eg`:

    require:
       earl-react as React
       /browser -> document
    
    require-macros:
       earl-react ->
          %, component
    
    component TodoList:
       render() =
          ul % enumerate(@props.items) each {i, item} ->
             li %
                key = i + item
                item
    
    component TodoApp:
       get-initial-state() =
          {items = {}, text = ""}
       render() =
          div %
             h3 % "TODO"
             TodoList % items = @state.items
             form %
                on-submit(e) =
                   e.prevent-default()
                   @set-state with {
                      items = @state.items.concat({@state.text})
                      text = ""
                   }
                input %
                   value = @state.text
                   on-change(e) =
                      @set-state with {text = e.target.value}
                button % 'Add #{@state.items.length + 1}'

    document.onload(e) =
       mount-node = document.get-element-by-id("mount")
       React.render(TodoApp %, mount-node)

Then you can compile with browserify and earlify:

    browserify -t earlify script.eg > bundle.js

Then include `bundle.js` on a page with some div with id "mount".
