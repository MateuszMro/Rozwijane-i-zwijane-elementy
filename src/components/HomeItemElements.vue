<script defer setup lang="js">
import $ from 'jquery'
$(document).ready(function() {

  var counter = 0
  var itemId = "item-" + counter
  // Pobieranie danych z localStorage
  var savedItems = localStorage.getItem("items")
  var items

  if (savedItems) {
    items = JSON.parse(savedItems)
  } else {
    // Generowanie 5 elementów domyślnych, jeśli brak danych w localStorage
    items = [
      { title: "Element 1", description: "Opis 1", collapsed: false },
      { title: "Element 2", description: "Opis 2", collapsed: false },
      { title: "Element 3", description: "Opis 3", collapsed: false },
      { title: "Element 4", description: "Opis 4", collapsed: false },
      { title: "Element 5", description: "Opis 5", collapsed: false }
    ]

    // Zapisywanie domyślnych elementów do localStorage
    localStorage.setItem("items", JSON.stringify(items))
  }

  // Renderowanie elementów
  renderItems(items)

  // Dodawanie nowego elementu
  $("#add-item").click(function() {
    counter++


    var newItem = { title: "Nowy element " + counter, description: "", collapsed: false }
    items.push(newItem)

    // Zapisywanie zmienionych danych do localStorage
    localStorage.setItem("items", JSON.stringify(items))

    renderItems(items)
  });

  // Renderowanie elementów
  function renderItems(items) {
    $("#item-container").empty()

    items.forEach(function(item, index) {
      var itemHeader = $("<div>")
          .addClass("item-header")
          .text(item.title)
          .click(function() {
            $(this).next(".item-content").slideToggle()

            // Aktualizowanie stanu zwinięcia/rozwinięcia
            items[index].collapsed = !items[index].collapsed
            localStorage.setItem("items", JSON.stringify(items))
          });

      var deleteButton = $("<button>")
          .addClass("delete-button")
          .text("Usuń")
          .click(function() {
            items.splice(index, 1)

            // Zapisywanie zmienionych danych do localStorage
            localStorage.setItem("items", JSON.stringify(items))

            renderItems(items)
          })

      var editButton= $("<button>")
          .addClass("edit-button")
          .text("Edytuj")
          .click(function (){
            var itemTitle = $(this).closest(".item").find(".item-title")
            var itemDescription = $(this).closest(".item").find(".item-description")

            itemTitle.attr("readonly",false)
            itemDescription.attr("readonly",false)
            itemTitle.addClass("edit-mode",false)
            itemDescription.addClass("edit-mode",false)
            itemTitle.focus()
          })

      var itemContent = $("<div>")
          .addClass("item-content")
          .toggle(!item.collapsed)
          .addClass("item-content")
          .toggle(!item.collapsed)
          .append(
              $("<label>").text("Tytuł:"),
              $("<input>")
                  .attr("type", "text")
                  .addClass("item-title")
                  .val(item.title)
                  .attr("readonly", true),
              $("<br>"),
              $("<label>").text("Opis:"),
              $("<textarea>")
                  .addClass("item-description")
                  .val(item.description)
                  .attr("readonly", true),
              $("<br>"),
              deleteButton,
              editButton
          )

      var item = $("<div>")
          .addClass("item")
          .attr("id", itemId)
          .append(itemHeader, itemContent)

      $("#item-container").append(item)
    })
  }
  $("#item-container").on("blur",".edit-mode",function (){
    var itemIndex = $(this).closest(".item").index()
    var itemTitle = $(this).closest(".item").find(".item-title").val()
    var itemDescription = $(this).closest('.item').find(".item-description").val()
    items[itemIndex].title = itemTitle
    items[itemIndex].description = itemDescription


    // Zapisywanie danych po edycji do localStorage
    localStorage.setItem("items",JSON.stringify(items))

    $(this).attr("readonly",true)
    $(this).removeClass("edit-mode")
  })
})

</script>

<template>

  <button id="add-item">Dodaj nowy element</button>
  <div id="item-container"></div>
</template>

<style>
button{
  margin-bottom: 20px;
  margin-right: 20px;
}
#add-item{
  position: absolute;
  top: 200px;
  left: 45%;
  width: 200px;

}
#item-container{
  position: absolute;
  top: 250px;
  left: 45%;
  width: 200px;

}
</style>