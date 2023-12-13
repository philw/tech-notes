# Fetching JSON Data

In the header create a span element with an id. This is where the JSON data will saved 
so that it can be used in On Render code event.

```html
<span id="guestslist" style="display: none">@[pf_custom:guestslist]</span>
```




```javascript
if(custom_tokens.refreshguests) {
	let weddingpid = auth_item_values["piid-weddings"]
	let filters = {filters: {"249160737": [intval(weddingpid)]}, "limit": 500, "sort_by":253711910, "sort_desc":false}
	let response = podio_api_raw_curl("/item/app/28364902/filter/", "POST", {}, filters)
	let items = response["items"]
	let guests = []
    
	foreach(items as item) {
	  let guest = {"pid": item["item_id"]}
	  foreach(item["fields"] as field) {
		  if(field["external_id"] == "full-name") {
			  guest["full-name"] = field["values"][0]["value"]
		  }
		  if(field["external_id"] == "guest-type-short") {
			  guest["guest-type-short"] = field["values"][0]["value"]
		  }
		  if(field["external_id"] == "guest-age-bracket") {
			  guest["guest-type"] = field["values"][0]["value"]["text"]
		  }
		  if(field["external_id"] == "table-number") {
			  guest["table-number"] = field["values"][0]["value"]["text"]
		  }
		  if(field["external_id"] == "allergy") {
			  guest["allergy"] = field["values"][0]["value"]["text"]
		  }
	  }

	  array_push(guests, guest)
	}

	custom_tokens.guestslist = json_encode(guests)
	custom_tokens.refreshguests = false
}
```