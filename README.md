# KuzQuality | Placeable items
## Adding "Place" options to your inventory system
___
## qb-inventory

### Step 1 `qb-inventory/html/ui.html`
add the following line between lines 36 and 37
```html
<div class="inv-option-item" id="item-kq-place"><p>PLACE</p></div>
```

Example:
`qb-inventory/html/ui.html`
```html
<div class="inv-options">
  <div class="inv-options-list">
    <input type="number" id="item-amount" class="inv-option-item" min=1 max="10000" placeholder="Amount" pattern="[0-9]" onfocus="this.value=''" placeholder="" oninput="validity.valid||(value='');"></input>
    <div class="inv-option-item" id="item-use"><p>USE</p></div>
    <div class="inv-option-item" id="item-kq-place"><p>PLACE</p></div>
    <div class="inv-option-item" id="item-give"><p>GIVE</p></div>
    <div class="inv-option-item" id="inv-close"><p>CLOSE</p></div>
  </div>
</div>
```

### Step 2 `qb-inventory/html/js/app.js`

At the very of the file. Add the following
```js

$("#item-kq-place").droppable({
    hoverClass: "button-hover",
    drop: function(event, ui) {
        setTimeout(function() {
            IsDragging = false;
        }, 300);
        fromData = ui.draggable.data("item");

        $.post(
            "https://kq_placeable_items/HookPlaceItem",
            JSON.stringify({
                item: fromData.name,
                size: 1,
            })
        );

        Inventory.Close();
    },
});
```
___
