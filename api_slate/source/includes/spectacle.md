# Spectacle
The Spectacle resource represents an item as it exists outside a [store](#store). Concretely, our spectacle catalog is the catalog from which store owners will have the ability to choose from when
building their online store.

## Frame Shapes

### cat-eye
![cat-eye](images/cateye.png)

### aviator
![aviator](images/aviator.png)

### squared
![squared](images/squared.png)

### round
![round](images/round.png)

### oval
![oval](images/oval.png)

### oversized
![oversized](images/oversized.png)

## Contours

* `full-rim`
* `half-rim`
* `rimless`


## Spectacle Model
Field | Required | Type | Default | Writeable | Filterable | Description
----- | -------- | ---- | ------- | --------- | ---------- | -----------
id | - | String | - | ✖️ | ✔️ | ID of the spectacle
created_at | - | String | Current Date | ✖️ | ✔️ | Creation date of the spectacle
modified_at | - | String | Current Date | ✖️ | ✔️ | Last modification of the spectacle
name | ✔️ | String | - | ✔️ | ✔️ | Display name of the spectacle
gender | ✔️ | String | - | ✔️ | ✔️ | Gender targeted by the spectacle
category | ✔️ | String | - | ✔️ | ✔️ | Spectacle category
color_code | ✔️ | String | - | ✔️ | ✔️ | Color code of the spectacle
sku | ✔️ | String | - | ✔️ | ✔️ | Manufacturer's spectacle SKU
colors | ✖️ | List\<String\> | [] | ✔️ | ✖️ | Colors present on the frame
material | ✖️ | String | `null` | ✔️ | ✔️ | Principal material constituting the frame (`null` when unknown)
shape | ✖️ | String | `null` | ✔️ | ✔️ | [Shape](#frame-shapes) of the frame (`null` when unknown)
contour | ✖️ | String | `null` | ✔️ | ✔️ | Type of [contour](#contours)
brand_name | ✖️ | String | `null` | ✔️ | ✔️ | Spectacle brand (`null` when unknown)
brand_url | ✖️ | String | `null` | ✔️ | ✔️ | Brand website (`null` when brand unknown)
manufacturer_name | ✖️ | String | `null` | ✔️ | ✔️ | Spectacle manufacturer (`null` when unknown)
manufacturer_url | ✖️ | String | `null` | ✔️ | ✔️ | Manufacturer website (`null` when manufacturer unknown)
images | ✖️ | Object | {} | ✔️ | ✖️ | Images of the spectacle
sizes | ✖️ | List\<Object\> | [] | ✔️ | ✖️ | Different spectacle dimensions
variants | ✖️ | List\<Object\> | [] | ✔️ | ✖️ | Different spectacle variants
is_premium | ✖️ | Boolean | `false` | ✔️ | ✔️ | Whether the spectacle's pictures are of premium quality
