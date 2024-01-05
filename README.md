# KuzQuality | Placeable items
## Adding "Place" options to your inventory system
___
## qb-inventory

### `qb-inventory/html/ui.html`
add the following line between lines 36 and 37
```html
<div class="inv-option-item" id="item-kq-place"><p>PLACE</p></div>
```

Example:
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

### `qb-inventory/html/js/app.js`

At the very of the file. Add the following
```js
$("#item-kq-place").droppable({
    hoverClass: "button-hover",
    drop: function(event, ui) {
        setTimeout(function() {
            IsDragging = false;
        }, 300);
        formData = ui.draggable.data("item");

        $.post(
            "https://kq_placeable_items/HookPlaceItem",
            JSON.stringify({
                item: formData.name,
                size: formData.amount || 1,
            })
        );

        Inventory.Close();
    },
});
```
___

## qs-inventory (Quasar)
qs-inventory will work automatically. No changes needed. Simply start this script after qs-inventory (Make sure that your qs-inventory is up to date)

## es_extended (default inventory)
> This might be a bit different depending on the version of your es_extended.
> Make sure to look at the surrounding lines of code around the new lines provider in the example

### `es_extended/client/functions.lua`
Add the following line to the line 819
```lua
table.insert(elements, {label = _U('place'), action = 'kq-place', type = data.current.type, value = data.current.value})
```

Example:
```lua
if data.current.usable then
  table.insert(elements, {label = _U('use'), action = 'use', type = data.current.type, value = data.current.value})
end

table.insert(elements, {label = _U('place'), action = 'kq-place', type = data.current.type, value = data.current.value})

if data.current.canRemove then
  if player ~= -1 and distance <= 3.0 then
	  table.insert(elements, {label = _U('give'), action = 'give', type = data.current.type, value = data.current.value})
  end

  table.insert(elements, {label = _U('remove'), action = 'remove', type = data.current.type, value = data.current.value})
end
```

Add the following lines under the line 941 (originally 940)

```lua
elseif data1.current.action == 'kq-place' then
  TriggerEvent('kq_materialize:client:hookPlaceItem', item, 1)
  ESX.UI.Menu.CloseAll()
```

Example:

```lua
elseif data1.current.action == 'use' then
  TriggerServerEvent('esx:useItem', item)
elseif data1.current.action == 'kq-place' then
  TriggerEvent('kq_materialize:client:hookPlaceItem', item, 1)
  ESX.UI.Menu.CloseAll()
elseif data1.current.action == 'return' then
  ESX.UI.Menu.CloseAll()
  ESX.ShowInventory()
```

### `es_extended/locales/en.lua` (or the locale you use)
Add the following to line to your locale

```lua
['place'] = 'place',
```
___

## lj-inventory
### `lj-inventory/html/js/app.js`
Add the following code at the bottom of the file
```js
$("#item-place").droppable({
  hoverClass: "button-hover",
  drop: function(event, ui) {
    setTimeout(function() {
      IsDragging = false;
    }, 300);
    fromData = ui.draggable.data("item");
    fromInventory = ui.draggable.parent().attr("data-inventory");
    amount = $("#item-amount").val();
    if (amount == 0) {
      amount = fromData.amount;
    }
    $.post(
      "http://kq_placeable_items/HookPlaceItem",
      JSON.stringify({
        item: fromData.name,
        size: parseInt(amount),
      })
    );
  },
});
```

### `lj-inventory/html/ui.html`

Add the following line between lines 63 and 64 (Under the "item-give" element)
```html
<div class="inv-option-item" id="item-place"><p>Place</p></div>
```

## esx_inventoryhud

### `esx_inventoryhud/html/ui.html`

Add the following line between line 34 and 35
```html
<div class="control" id="kq-place">Place</div>
```

Example:
```html
<div class="controls-div">
	<input type="number" class="control" id="count" value="1">
	<div class="control" id="use"></div>
	<div class="control" id="kq-place">Place</div>
	<div class="control" id="give"></div>
        <div class="control" id="drop"></div>
</div>
```

### `esx_inventoryhud/html/js/inventory.js`

Add the following on the line 259

```js
$('#kq-place').droppable({
        hoverClass: 'hoverControl',
        drop: function (event, ui) {
            itemData = ui.draggable.data("item");

            if (itemData == undefined) {
                return;
            }

            itemInventory = ui.draggable.data("inventory");

            if (itemInventory == undefined || itemInventory == "second") {
                return;
            }

	    closeInventory();
	    $.post("http://kq_placeable_items/HookPlaceItem", JSON.stringify({
            	item: itemData.name,
		size: itemData.amount || 1,
	    }));
        }
    });
```

___
